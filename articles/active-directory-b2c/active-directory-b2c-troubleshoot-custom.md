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
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Shromažďování protokolů.

Tento článek popisuje kroky pro shromažďování protokolů z Azure AD B2C, takže lze diagnostikovat problémy s vlastní zásady.

>[!NOTE]
>V současné době hello protokoly podrobné aktivity, které jsou zde popsané navržených **pouze** tooaid v vývoj vlastních zásad. Nepoužívejte režimu pro vývoj v produkčním prostředí.  Protokoly shromažďovat všechny deklarace identity odeslat tooand od zprostředkovatelů identity hello během vývoje.  Pokud se používá v produkčním prostředí, hello developer odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu hello statistiky aplikace, kterou vlastní.  Tyto podrobné protokoly jsou shromažďovány pouze při hello zásad je umístěn na **režimu pro vývoj**.


## <a name="use-application-insights"></a>Pomocí Application Insights

Azure AD B2C podporuje funkci pro odesílání dat tooApplication statistiky.  Application Insights poskytuje způsob toodiagnose výjimky a vizualizovat problémy s výkonem aplikace.

### <a name="setup-application-insights"></a>Instalační program Application Insights

1. Přejděte toohello [portál Azure](https://portal.azure.com). Zkontrolujte, zda že jste v klientovi hello ve vašem předplatném Azure (ne vašeho klienta Azure AD B2C).
1. Klikněte na tlačítko **+ nový** v levé navigační nabídce hello.
1. Vyhledejte a vyberte **Application Insights**, pak klikněte na tlačítko **vytvořit**.
1. Vyplňte formulář hello a klikněte na tlačítko **vytvořit**. Vyberte **Obecné** pro hello **typ aplikace**.
1. Po vytvoření hello prostředků, otevřete prostředek Application Insights hello.
1. Najít **vlastnosti** v hello nabídky na levé straně a klikněte na jeho.
1. Kopírování hello **klíč instrumentace** a uložit pro další části hello.

### <a name="set-up-hello-custom-policy"></a>Nastavení vlastních zásad pro hello

1. Otevřete soubor RP hello (například SignUpOrSignin.xml).
1. Přidejte následující atributy toohello hello `<TrustFrameworkPolicy>` element:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Pokud již neexistuje, přidejte podřízený uzel `<UserJourneyBehaviors>` toohello `<RelyingParty>` uzlu. Musí být umístěna bezprostředně po hello`<DefaultUserJourney ReferenceId="YourPolicyName" />`
2. Přidejte následující uzlu jako podřízenou hello hello `<UserJourneyBehaviors>` elementu. Ujistěte se, že tooreplace `{Your Application Insights Key}` s hello **klíč instrumentace** získaný ze služby Application Insights v předchozí části hello.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"`informuje ApplicationInsights tooexpedite hello telemetrická data prostřednictvím kanálu zpracování hello, vhodné pro vývoj, ale omezené na vysokou svazky.
  * `ClientEnabled="true"`odešle hello ApplicationInsights klientský skript pro sledování zobrazení a na straně klienta chyby stránky (není potřeba).
  * `ServerEnabled="true"`odešle hello existující JSON UserJourneyRecorder jako vlastní události tooApplication statistiky.
Ukázka:

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

3. Nahrajte zásady hello.

### <a name="see-hello-logs-in-application-insights"></a>Zjistit hello protokoly ve službě Application Insights

>[!NOTE]
> Je malou zpoždění (méně než pět minut), než budete moct vidět nové protokoly ve službě Application Insights.

1. Otevřete prostředek Application Insights hello, který jste vytvořili v hello [portál Azure](https://portal.azure.com).
1. V hello **přehled** nabídky, klikněte na **Analytics**.
1. Otevřete novou kartu ve službě Application Insights.
1. Tady je seznam dotazů, že které můžete použít toosee hello protokoly

| Dotaz | Popis |
|---------------------|--------------------|
Trasování | Zobrazit všechny hello protokoly služby Azure AD B2C |
trasování \| kde časové razítko > ago(1d) | Zobrazit všechny protokoly hello vygenerované pomocí Azure AD B2C hello poslední den

položky Hello může trvat dlouho.  Export tooCSV obrázek otevřete.

Další informace o nástroj pro analýzu hello [zde](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>Hello komunity vyvinula uživatel cesty prohlížeč toohelp identity vývojáři.  Není podporovaný společností Microsoft a k dispozici výhradně jako-je.  Přečte z vaší instance služby Application Insights a obsahuje také struktura zobrazení událostí cesty hello uživatele.  Získat hello zdrojového kódu a nasaďte ho ve vlastní řešení.

>[!NOTE]
>V současné době hello protokoly podrobné aktivity, které jsou zde popsané navržených **pouze** tooaid v vývoj vlastních zásad. Nepoužívejte režimu pro vývoj v produkčním prostředí.  Protokoly shromažďovat všechny deklarace identity odeslat tooand od zprostředkovatelů identity hello během vývoje.  Pokud se používá v produkčním prostředí, hello developer odpovědnost pro PII (soukromě osobní informace) shromážděné v protokolu hello statistiky aplikace, kterou vlastní.  Tyto podrobné protokoly jsou shromažďovány pouze při hello zásad je umístěn na **režimu pro vývoj**.

[Úložiště Github pro nepodporovaný – ukázky vlastních zásad a související nástroje](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Další kroky

Prozkoumejte hello data v Application Insights toohelp porozumíte jak hello Identity Framework prostředí základní B2C funguje toodeliver narazí vaši vlastní identitu.
