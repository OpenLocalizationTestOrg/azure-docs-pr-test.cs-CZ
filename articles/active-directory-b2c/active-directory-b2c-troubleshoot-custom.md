---
title: "Application Insights k řešení potíží se zásadami vlastní – Azure AD B2C | Microsoft Docs"
description: "Postup instalace Application Insights pro sledování spuštění vlastní zásady"
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
ms.openlocfilehash: 8c79df33cd5f04f490e2cc6372f7e8ac1c4d9bbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="4e4a8-103">Azure Active Directory B2C: Shromažďování protokolů.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="4e4a8-104">Tento článek popisuje kroky pro shromažďování protokolů z Azure AD B2C, takže lze diagnostikovat problémy s vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="4e4a8-105">V současné době jsou navrženy protokoly podrobné aktivity, které jsou zde popsané **pouze** a usnadňuje vývoj vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-105">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="4e4a8-106">Nepoužívejte režimu pro vývoj v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="4e4a8-107">Protokoly shromažďovat všechny deklarace identity posílané do a z poskytovatelů identit během vývoje.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-107">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="4e4a8-108">Pokud se používá v produkčním prostředí, vývojář odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu statistiky aplikace, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-108">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="4e4a8-109">Tyto podrobné protokoly jsou shromažďovány pouze, pokud zásady je umístěn na **režimu pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-109">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="4e4a8-110">Pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="4e4a8-110">Use Application Insights</span></span>

<span data-ttu-id="4e4a8-111">Azure AD B2C podporuje funkci pro odesílání dat do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-111">Azure AD B2C supports a feature for sending data to Application Insights.</span></span>  <span data-ttu-id="4e4a8-112">Application Insights umožňuje diagnostikovat výjimky a vizualizovat problémy s výkonem aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-112">Application Insights provides a way to diagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="4e4a8-113">Instalační program Application Insights</span><span class="sxs-lookup"><span data-stu-id="4e4a8-113">Setup Application Insights</span></span>

1. <span data-ttu-id="4e4a8-114">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-114">Go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4e4a8-115">Zkontrolujte, zda že jste v klientovi s předplatným Azure (ne vašeho klienta Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-115">Ensure you are in the tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="4e4a8-116">Klikněte na tlačítko **+ nový** v levé navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-116">Click **+ New** in the left-hand navigation menu.</span></span>
1. <span data-ttu-id="4e4a8-117">Vyhledejte a vyberte **Application Insights**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="4e4a8-118">Vyplňte formulář a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-118">Complete the form and click **Create**.</span></span> <span data-ttu-id="4e4a8-119">Vyberte **Obecné** pro **typ aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-119">Select **General** for the **Application Type**.</span></span>
1. <span data-ttu-id="4e4a8-120">Po vytvoření prostředku, otevřete prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-120">Once the resource has been created, open the Application Insights resource.</span></span>
1. <span data-ttu-id="4e4a8-121">Najít **vlastnosti** v levé nabídce a klepněte na něj.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-121">Find **Properties** in the left-menu, and click on it.</span></span>
1. <span data-ttu-id="4e4a8-122">Kopírování **klíč instrumentace** a uložit pro další části.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-122">Copy the **Instrumentation Key** and save it for the next section.</span></span>

### <a name="set-up-the-custom-policy"></a><span data-ttu-id="4e4a8-123">Nastavení vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="4e4a8-123">Set up the custom policy</span></span>

1. <span data-ttu-id="4e4a8-124">Otevřete soubor RP (například SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-124">Open the RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="4e4a8-125">Přidejte následující atributy, které se `<TrustFrameworkPolicy>` element:</span><span class="sxs-lookup"><span data-stu-id="4e4a8-125">Add the following attributes to the `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="4e4a8-126">Pokud již neexistuje, přidejte podřízený uzel `<UserJourneyBehaviors>` k `<RelyingParty>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` to the `<RelyingParty>` node.</span></span> <span data-ttu-id="4e4a8-127">Musí být umístěna bezprostředně po`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="4e4a8-127">It must be located immediately after the `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="4e4a8-128">Přidejte následující uzel jako podřízený `<UserJourneyBehaviors>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-128">Add the following node as a child of the `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="4e4a8-129">Nezapomeňte nahradit `{Your Application Insights Key}` s **klíč instrumentace** získaný ze služby Application Insights v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-129">Make sure to replace `{Your Application Insights Key}` with the **Instrumentation Key** that you obtained from Application Insights in the previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="4e4a8-130">`DeveloperMode="true"`informuje ApplicationInsights urychlit telemetrie skrz kanál zpracování, vhodné pro vývoj, ale omezené na vysokou svazky.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-130">`DeveloperMode="true"` tells ApplicationInsights to expedite the telemetry through the processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="4e4a8-131">`ClientEnabled="true"`odešle klientský skript ApplicationInsights pro sledování zobrazení a na straně klienta chyby stránky (není potřeba).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-131">`ClientEnabled="true"` sends the ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="4e4a8-132">`ServerEnabled="true"`odešle existující JSON UserJourneyRecorder jako vlastní události Application insights.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-132">`ServerEnabled="true"` sends the existing UserJourneyRecorder JSON as a custom event to Application Insights.</span></span>
<span data-ttu-id="4e4a8-133">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="4e4a8-133">Sample:</span></span>

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

3. <span data-ttu-id="4e4a8-134">Nahrajte zásady.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-134">Upload the policy.</span></span>

### <a name="see-the-logs-in-application-insights"></a><span data-ttu-id="4e4a8-135">V protokolech ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="4e4a8-135">See the logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="4e4a8-136">Je malou zpoždění (méně než pět minut), než budete moct vidět nové protokoly ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="4e4a8-137">Otevřete prostředek Application Insights, který jste vytvořili v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-137">Open the Application Insights resource that you created in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="4e4a8-138">V **přehled** nabídky, klikněte na **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-138">In the **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="4e4a8-139">Otevřete novou kartu ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="4e4a8-140">Tady je seznam dotazů, které můžete použít k najdete v souborech protokolů</span><span class="sxs-lookup"><span data-stu-id="4e4a8-140">Here is a list of queries you can use to see the logs</span></span>

| <span data-ttu-id="4e4a8-141">Dotaz</span><span class="sxs-lookup"><span data-stu-id="4e4a8-141">Query</span></span> | <span data-ttu-id="4e4a8-142">Popis</span><span class="sxs-lookup"><span data-stu-id="4e4a8-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="4e4a8-143">Trasování</span><span class="sxs-lookup"><span data-stu-id="4e4a8-143">traces</span></span> | <span data-ttu-id="4e4a8-144">Zobrazit všechny protokoly služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="4e4a8-144">See all of the logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="4e4a8-145">trasování \\</span><span class="sxs-lookup"><span data-stu-id="4e4a8-145">traces \\</span></span>| <span data-ttu-id="4e4a8-146">kde časové razítko > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="4e4a8-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="4e4a8-147">Zobrazit všechny protokoly služby Azure AD B2C pro poslední den</span><span class="sxs-lookup"><span data-stu-id="4e4a8-147">See all of the logs generated by Azure AD B2C for the last day</span></span>

<span data-ttu-id="4e4a8-148">Položky může trvat dlouho.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-148">The entries may be long.</span></span>  <span data-ttu-id="4e4a8-149">Obrázek otevřete exportovat do souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-149">Export to CSV for a closer look.</span></span>

<span data-ttu-id="4e4a8-150">Další informace o nástroj pro analýzu [zde](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="4e4a8-150">You can learn more about the Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="4e4a8-151">Komunita vyvinula prohlížeč cesty uživatele, což vývojářům identity.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-151">The community has developed a user journey viewer to help identity developers.</span></span>  <span data-ttu-id="4e4a8-152">Není podporovaný společností Microsoft a k dispozici výhradně jako-je.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="4e4a8-153">Přečte z vaší instance služby Application Insights a poskytuje také struktura zobrazení uživatele cesty události.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-153">It reads from your Application Insights instance and provides a well-structure view of the user journey events.</span></span>  <span data-ttu-id="4e4a8-154">Získat zdrojový kód a nasaďte ho ve vlastní řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-154">You obtain the source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="4e4a8-155">V současné době jsou navrženy protokoly podrobné aktivity, které jsou zde popsané **pouze** a usnadňuje vývoj vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-155">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="4e4a8-156">Nepoužívejte režimu pro vývoj v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-156">Do not use development mode in production.</span></span>  <span data-ttu-id="4e4a8-157">Protokoly shromažďovat všechny deklarace identity posílané do a z poskytovatelů identit během vývoje.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-157">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="4e4a8-158">Pokud se používá v produkčním prostředí, vývojář odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu statistiky aplikace, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-158">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="4e4a8-159">Tyto podrobné protokoly jsou shromažďovány pouze, pokud zásady je umístěn na **režimu pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-159">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="4e4a8-160">Úložiště Github pro nepodporovaný – ukázky vlastních zásad a související nástroje</span><span class="sxs-lookup"><span data-stu-id="4e4a8-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="4e4a8-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e4a8-161">Next Steps</span></span>

<span data-ttu-id="4e4a8-162">Prozkoumejte data ve službě Application Insights vám pomohou pochopit, jak se vyskytne rozhraní prostředí Identity základní B2C funguje k poskytování vlastní identity.</span><span class="sxs-lookup"><span data-stu-id="4e4a8-162">Explore the data in Application Insights to help you understand how the Identity Experience Framework underlying B2C works to deliver your own identity experiences.</span></span>