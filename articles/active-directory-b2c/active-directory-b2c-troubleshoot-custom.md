---
title: "Application Insights tootroubleshoot vlastní zásady – Azure AD B2C | Microsoft Docs"
description: "jak toosetup Application Insights tootrace hello spouštění vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: c02d7178512c7f9e022385371c3effd4f8cb7726
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="5ccd9-103">Azure Active Directory B2C: Shromažďování protokolů.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="5ccd9-104">Tento článek popisuje kroky pro shromažďování protokolů z Azure AD B2C, takže lze diagnostikovat problémy s vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="5ccd9-105">V současné době hello protokoly podrobné aktivity, které jsou zde popsané navržených **pouze** tooaid v vývoj vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-105">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="5ccd9-106">Nepoužívejte režimu pro vývoj v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="5ccd9-107">Protokoly shromažďovat všechny deklarace identity odeslat tooand od zprostředkovatelů identity hello během vývoje.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-107">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="5ccd9-108">Pokud se používá v produkčním prostředí, hello developer odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu hello statistiky aplikace, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-108">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="5ccd9-109">Tyto podrobné protokoly jsou shromažďovány pouze při hello zásad je umístěn na **režimu pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-109">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="5ccd9-110">Pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="5ccd9-110">Use Application Insights</span></span>

<span data-ttu-id="5ccd9-111">Azure AD B2C podporuje funkci pro odesílání dat tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-111">Azure AD B2C supports a feature for sending data tooApplication Insights.</span></span>  <span data-ttu-id="5ccd9-112">Application Insights poskytuje způsob toodiagnose výjimky a vizualizovat problémy s výkonem aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-112">Application Insights provides a way toodiagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="5ccd9-113">Instalační program Application Insights</span><span class="sxs-lookup"><span data-stu-id="5ccd9-113">Setup Application Insights</span></span>

1. <span data-ttu-id="5ccd9-114">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-114">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5ccd9-115">Zkontrolujte, zda že jste v klientovi hello ve vašem předplatném Azure (ne vašeho klienta Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-115">Ensure you are in hello tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="5ccd9-116">Klikněte na tlačítko **+ nový** v levé navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-116">Click **+ New** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="5ccd9-117">Vyhledejte a vyberte **Application Insights**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="5ccd9-118">Vyplňte formulář hello a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-118">Complete hello form and click **Create**.</span></span> <span data-ttu-id="5ccd9-119">Vyberte **Obecné** pro hello **typ aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-119">Select **General** for hello **Application Type**.</span></span>
1. <span data-ttu-id="5ccd9-120">Po vytvoření hello prostředků, otevřete prostředek Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-120">Once hello resource has been created, open hello Application Insights resource.</span></span>
1. <span data-ttu-id="5ccd9-121">Najít **vlastnosti** v hello nabídky na levé straně a klikněte na jeho.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-121">Find **Properties** in hello left-menu, and click on it.</span></span>
1. <span data-ttu-id="5ccd9-122">Kopírování hello **klíč instrumentace** a uložit pro další části hello.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-122">Copy hello **Instrumentation Key** and save it for hello next section.</span></span>

### <a name="set-up-hello-custom-policy"></a><span data-ttu-id="5ccd9-123">Nastavení vlastních zásad pro hello</span><span class="sxs-lookup"><span data-stu-id="5ccd9-123">Set up hello custom policy</span></span>

1. <span data-ttu-id="5ccd9-124">Otevřete soubor RP hello (například SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-124">Open hello RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="5ccd9-125">Přidejte následující atributy toohello hello `<TrustFrameworkPolicy>` element:</span><span class="sxs-lookup"><span data-stu-id="5ccd9-125">Add hello following attributes toohello `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="5ccd9-126">Pokud již neexistuje, přidejte podřízený uzel `<UserJourneyBehaviors>` toohello `<RelyingParty>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` toohello `<RelyingParty>` node.</span></span> <span data-ttu-id="5ccd9-127">Musí být umístěna bezprostředně po hello`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="5ccd9-127">It must be located immediately after hello `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="5ccd9-128">Přidejte následující uzlu jako podřízenou hello hello `<UserJourneyBehaviors>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-128">Add hello following node as a child of hello `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="5ccd9-129">Ujistěte se, že tooreplace `{Your Application Insights Key}` s hello **klíč instrumentace** získaný ze služby Application Insights v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-129">Make sure tooreplace `{Your Application Insights Key}` with hello **Instrumentation Key** that you obtained from Application Insights in hello previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="5ccd9-130">`DeveloperMode="true"`informuje ApplicationInsights tooexpedite hello telemetrická data prostřednictvím kanálu zpracování hello, vhodné pro vývoj, ale omezené na vysokou svazky.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-130">`DeveloperMode="true"` tells ApplicationInsights tooexpedite hello telemetry through hello processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="5ccd9-131">`ClientEnabled="true"`odešle hello ApplicationInsights klientský skript pro sledování zobrazení a na straně klienta chyby stránky (není potřeba).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-131">`ClientEnabled="true"` sends hello ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="5ccd9-132">`ServerEnabled="true"`odešle hello existující JSON UserJourneyRecorder jako vlastní události tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-132">`ServerEnabled="true"` sends hello existing UserJourneyRecorder JSON as a custom event tooApplication Insights.</span></span>
<span data-ttu-id="5ccd9-133">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="5ccd9-133">Sample:</span></span>

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="YourPolicyName" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. <span data-ttu-id="5ccd9-134">Nahrajte zásady hello.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-134">Upload hello policy.</span></span>

### <a name="see-hello-logs-in-application-insights"></a><span data-ttu-id="5ccd9-135">Zjistit hello protokoly ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="5ccd9-135">See hello logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="5ccd9-136">Je malou zpoždění (méně než pět minut), než budete moct vidět nové protokoly ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="5ccd9-137">Otevřete prostředek Application Insights hello, který jste vytvořili v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-137">Open hello Application Insights resource that you created in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="5ccd9-138">V hello **přehled** nabídky, klikněte na **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-138">In hello **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="5ccd9-139">Otevřete novou kartu ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="5ccd9-140">Tady je seznam dotazů, že které můžete použít toosee hello protokoly</span><span class="sxs-lookup"><span data-stu-id="5ccd9-140">Here is a list of queries you can use toosee hello logs</span></span>

| <span data-ttu-id="5ccd9-141">Dotaz</span><span class="sxs-lookup"><span data-stu-id="5ccd9-141">Query</span></span> | <span data-ttu-id="5ccd9-142">Popis</span><span class="sxs-lookup"><span data-stu-id="5ccd9-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="5ccd9-143">Trasování</span><span class="sxs-lookup"><span data-stu-id="5ccd9-143">traces</span></span> | <span data-ttu-id="5ccd9-144">Zobrazit všechny hello protokoly služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5ccd9-144">See all of hello logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="5ccd9-145">trasování \\</span><span class="sxs-lookup"><span data-stu-id="5ccd9-145">traces \\</span></span>| <span data-ttu-id="5ccd9-146">kde časové razítko > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="5ccd9-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="5ccd9-147">Zobrazit všechny protokoly hello vygenerované pomocí Azure AD B2C hello poslední den</span><span class="sxs-lookup"><span data-stu-id="5ccd9-147">See all of hello logs generated by Azure AD B2C for hello last day</span></span>

<span data-ttu-id="5ccd9-148">položky Hello může trvat dlouho.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-148">hello entries may be long.</span></span>  <span data-ttu-id="5ccd9-149">Export tooCSV obrázek otevřete.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-149">Export tooCSV for a closer look.</span></span>

<span data-ttu-id="5ccd9-150">Další informace o nástroj pro analýzu hello [zde](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="5ccd9-150">You can learn more about hello Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="5ccd9-151">Hello komunity vyvinula uživatel cesty prohlížeč toohelp identity vývojáři.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-151">hello community has developed a user journey viewer toohelp identity developers.</span></span>  <span data-ttu-id="5ccd9-152">Není podporovaný společností Microsoft a k dispozici výhradně jako-je.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="5ccd9-153">Přečte z vaší instance služby Application Insights a obsahuje také struktura zobrazení událostí cesty hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-153">It reads from your Application Insights instance and provides a well-structure view of hello user journey events.</span></span>  <span data-ttu-id="5ccd9-154">Získat hello zdrojového kódu a nasaďte ho ve vlastní řešení.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-154">You obtain hello source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="5ccd9-155">V současné době hello protokoly podrobné aktivity, které jsou zde popsané navržených **pouze** tooaid v vývoj vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-155">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="5ccd9-156">Nepoužívejte režimu pro vývoj v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-156">Do not use development mode in production.</span></span>  <span data-ttu-id="5ccd9-157">Protokoly shromažďovat všechny deklarace identity odeslat tooand od zprostředkovatelů identity hello během vývoje.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-157">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="5ccd9-158">Pokud se používá v produkčním prostředí, hello developer odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu hello statistiky aplikace, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-158">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="5ccd9-159">Tyto podrobné protokoly jsou shromažďovány pouze při hello zásad je umístěn na **režimu pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-159">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="5ccd9-160">Úložiště Github pro nepodporovaný – ukázky vlastních zásad a související nástroje</span><span class="sxs-lookup"><span data-stu-id="5ccd9-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="5ccd9-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ccd9-161">Next Steps</span></span>

<span data-ttu-id="5ccd9-162">Prozkoumejte hello data v Application Insights toohelp porozumíte jak hello Identity Framework prostředí základní B2C funguje toodeliver narazí vaši vlastní identitu.</span><span class="sxs-lookup"><span data-stu-id="5ccd9-162">Explore hello data in Application Insights toohelp you understand how hello Identity Experience Framework underlying B2C works toodeliver your own identity experiences.</span></span>
