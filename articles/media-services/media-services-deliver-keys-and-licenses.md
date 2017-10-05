---
title: "Pomocí Azure Media Services pro doručování licencí DRM nebo klíčů AES"
description: "Tento článek popisuje, jak lze pomocí Azure Media Services (AMS) k poskytování PlayReady nebo Widevine licencí a klíčů AES ale udělá zbytek (kódování, šifrování, streamování) pomocí místních serverů."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 8546c2c1-430b-4254-a88d-4436a83f9192
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 263a381dc72105eea60ad9b39434599ff04a4531
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-media-services-to-deliver-drm-licenses-or-aes-keys"></a><span data-ttu-id="be375-103">Pomocí Azure Media Services pro doručování licencí DRM nebo klíčů AES</span><span class="sxs-lookup"><span data-stu-id="be375-103">Use Azure Media Services to deliver DRM licenses or AES keys</span></span>
<span data-ttu-id="be375-104">Azure Media Services (AMS) umožňuje ingestovat, kódovat, přidat ochranu obsahu a Streamovat obsah (viz [to](media-services-protect-with-drm.md) článku podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="be375-104">Azure Media Services (AMS) enables you to ingest, encode, add content protection, and stream your content (see [this](media-services-protect-with-drm.md) article for details).</span></span> <span data-ttu-id="be375-105">Existují však zákazníci, kteří pouze chcete použít AMS doručování licencí nebo klíče a provádět kódování, šifrování a streamování pomocí jejich místních serverů.</span><span class="sxs-lookup"><span data-stu-id="be375-105">However, there are customers who only want to use AMS to deliver licenses and/or keys and do encoding, encrypting and streaming using their on-premises servers.</span></span> <span data-ttu-id="be375-106">Tento článek popisuje, jak můžete použít AMS doručování licencí PlayReady nebo Widevine, ale udělá zbytek s vašimi místními servery.</span><span class="sxs-lookup"><span data-stu-id="be375-106">This article describes how you can use AMS to deliver PlayReady and/or Widevine licenses but do the rest with your on-premises servers.</span></span> 

## <a name="overview"></a><span data-ttu-id="be375-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="be375-107">Overview</span></span>
<span data-ttu-id="be375-108">Služba Media Services poskytuje službu k doručování PlayReady a Widevine DRM licence a klíčů AES-128.</span><span class="sxs-lookup"><span data-stu-id="be375-108">Media Services provides a service for delivering PlayReady and Widevine DRM licenses and AES-128 keys.</span></span> <span data-ttu-id="be375-109">Služba Media Services také poskytuje rozhraní API umožňující nakonfigurovat práva a omezení, které chcete použít pro modul runtime DRM vynucovat, když uživatel přehrává DRM chráněný obsah.</span><span class="sxs-lookup"><span data-stu-id="be375-109">Media Services also provides APIs that let you configure the rights and restrictions that you want for the DRM runtime to enforce when a user plays back the DRM protected content.</span></span> <span data-ttu-id="be375-110">Když si uživatel vyžádá chráněný obsah, aplikace přehrávače si vyžádá licenci z licenční služby AMS.</span><span class="sxs-lookup"><span data-stu-id="be375-110">When a user requests the protected content, the player application will request a license from the AMS license service.</span></span> <span data-ttu-id="be375-111">Licenční služba AMS bude vystavovat licence přehrávači (Pokud je povoleno).</span><span class="sxs-lookup"><span data-stu-id="be375-111">The AMS license service will issue the license to the player (if it is authorized).</span></span> <span data-ttu-id="be375-112">Licence PlayReady a Widevine obsahuje dešifrovací klíč, který lze použít klientský přehrávač dešifrovat a Streamovat obsah.</span><span class="sxs-lookup"><span data-stu-id="be375-112">The PlayReady and Widevine licenses contain the decryption key that can be used by the client player to decrypt and stream the content.</span></span>

<span data-ttu-id="be375-113">Služba Media Services podporuje více způsobů autorizace uživatelů, kteří žádají licence nebo o klíč.</span><span class="sxs-lookup"><span data-stu-id="be375-113">Media Services supports multiple ways of authorizing users who make license or key requests.</span></span> <span data-ttu-id="be375-114">Nakonfigurujte zásady autorizace klíče obsahu a zásady může mít jeden nebo více omezení: otevření nebo token omezení.</span><span class="sxs-lookup"><span data-stu-id="be375-114">You configure the content key's authorization policy and the policy could have one or more restrictions: open or token restriction.</span></span> <span data-ttu-id="be375-115">Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="be375-115">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="be375-116">Služba Media Services podporuje tokeny ve formátu jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT).</span><span class="sxs-lookup"><span data-stu-id="be375-116">Media Services supports tokens in the Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span>

<span data-ttu-id="be375-117">Následující diagram znázorňuje hlavní kroky, že které je třeba provést pomocí AMS doručování licencí PlayReady nebo Widevine, ale udělá zbytek s vašimi místními servery.</span><span class="sxs-lookup"><span data-stu-id="be375-117">The following diagram shows the main steps you need to take to use AMS to deliver PlayReady and/or Widevine licenses but do the rest with your on-premises servers.</span></span>

![Ochrana technologií PlayReady](./media/media-services-deliver-keys-and-licenses/media-services-diagram1.png)

## <a name="download-sample"></a><span data-ttu-id="be375-119">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="be375-119">Download sample</span></span>
<span data-ttu-id="be375-120">Ukázku popsanou v tomto článku si můžete stáhnout [tady](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).</span><span class="sxs-lookup"><span data-stu-id="be375-120">You can download the sample described in this article from [here](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="be375-121">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="be375-121">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="be375-122">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="be375-122">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="be375-123">Do části **appSettings** definované ve vašem souboru app.config přidejte následující elementy:</span><span class="sxs-lookup"><span data-stu-id="be375-123">Add the following elements to **appSettings** defined in your app.config file:</span></span>

    <span data-ttu-id="be375-124"><add key="Issuer" value="http://testacs.com"/> <add key="Audience" value="urn:test"/></span><span class="sxs-lookup"><span data-stu-id="be375-124"><add key="Issuer" value="http://testacs.com"/> <add key="Audience" value="urn:test"/></span></span>

## <a name="net-code-example"></a><span data-ttu-id="be375-125">Příklad kódu rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="be375-125">.NET code example</span></span>

<span data-ttu-id="be375-126">Následující příklad kódu ukazuje, jak vytvořit běžné klíč obsahu a získání PlayReady nebo Widevine získání adres URL licence.</span><span class="sxs-lookup"><span data-stu-id="be375-126">The following code example shows how to create a common content key and get PlayReady or Widevine license acquisition URLs.</span></span> <span data-ttu-id="be375-127">Je nutné získat následující informace z AMS a nakonfigurovat na místním serveru: **klíč obsahu**, **id klíče**, **adresu URL pro získání licence**.</span><span class="sxs-lookup"><span data-stu-id="be375-127">You need to get the following pieces of information from AMS and configure your on-premises server: **content key**, **key id**, **license acquisition URL**.</span></span> <span data-ttu-id="be375-128">Jakmile nakonfigurujete na místním serveru, může datového proudu ze serveru streamování.</span><span class="sxs-lookup"><span data-stu-id="be375-128">Once you configure your on-premises server, you could stream from your own streaming server.</span></span> <span data-ttu-id="be375-129">Vzhledem k tomu, že body šifrovaného datového proudu k AMS licenční server, přehrávače si vyžádá licenci z AMS.</span><span class="sxs-lookup"><span data-stu-id="be375-129">Since the encrypted stream points to AMS license server, your player will request a license from AMS.</span></span> <span data-ttu-id="be375-130">Pokud si zvolíte ověřování tokenem, licenční server AMS ověří token, které jste poslali prostřednictvím protokolu HTTPS a (Pokud je to platný) získávat licence zpět do přehrávače.</span><span class="sxs-lookup"><span data-stu-id="be375-130">If you choose token authentication, the AMS license server will validate the token you sent through HTTPS and (if valid) will deliver the license back to your player.</span></span> <span data-ttu-id="be375-131">(Příklad kódu pouze ukazuje postup vytvoření běžné klíč obsahu a získání PlayReady nebo Widevine získání adres URL licence.</span><span class="sxs-lookup"><span data-stu-id="be375-131">(The code example only shows how to create a common content key and  get PlayReady or Widevine license acquisition URLs.</span></span> <span data-ttu-id="be375-132">Pokud chcete klíče doručení AES-128, budete muset vytvořit klíč obsahu pomocí obálky a získat adresu URL, získávání klíčů a [to](media-services-protect-with-aes128.md) článek ukazuje, jak to udělat).</span><span class="sxs-lookup"><span data-stu-id="be375-132">If you want to delivery AES-128 keys, you need to create an envelope content key and get a key acquisition URL and [this](media-services-protect-with-aes128.md) article shows how to do it).</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DeliverDRMLicenses
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            private static readonly Uri _sampleIssuer =
                new Uri(ConfigurationManager.AppSettings["Issuer"]);
            private static readonly Uri _sampleAudience =
                new Uri(ConfigurationManager.AppSettings["Audience"]);

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                bool tokenRestriction = true;
                string tokenTemplateString = null;


                IContentKey key = CreateCommonTypeContentKey();

                // Print out the key ID and Key in base64 string format
                Console.WriteLine("Created key {0} with key value {1} ",
                    key.Id, System.Convert.ToBase64String(key.GetClearKeyValue()));

                Console.WriteLine("PlayReady License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));

                Console.WriteLine("Widevine License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine));

                if (tokenRestriction)
                    tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                else
                    AddOpenAuthorizationPolicy(key);

                Console.WriteLine("Added authorization policy: {0}",
                    key.AuthorizationPolicyId);
                Console.WriteLine();
                Console.ReadLine();
            }

            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {

                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
                {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                };

                // Configure PlayReady and Widevine license templates.
                string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

                IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, PlayReadyLicenseTemplate);

                IContentKeyAuthorizationPolicyOption WidevinePolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.Widevine,
                        restrictions, WidevineLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
            {
                string tokenTemplateString = GenerateTokenRequirements();

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
                {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                };

                // Configure PlayReady and Widevine license templates.
                string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

                IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, PlayReadyLicenseTemplate);

                IContentKeyAuthorizationPolicyOption WidevinePolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                        ContentKeyDeliveryType.Widevine,
                            restrictions, WidevineLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with token restrictions").
                            Result;

                contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

                // Associate the content key authorization policy with the content key
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;

                return tokenTemplateString;
            }

            static private string GenerateTokenRequirements()
            {
                TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

                template.PrimaryVerificationKey = new SymmetricVerificationKey();
                template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                template.Audience = _sampleAudience.ToString();
                template.Issuer = _sampleIssuer.ToString();
                template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

                return TokenRestrictionTemplateSerializer.Serialize(template);
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.

                //The PlayReadyLicenseResponseTemplate class represents the template 
                //for the response sent back to the end user. 
                //It contains a field for a custom data string between the license server 
                //and the application (may be useful for custom app logic) 
                //as well as a list of one or more license templates.

                PlayReadyLicenseResponseTemplate responseTemplate =
                    new PlayReadyLicenseResponseTemplate();

                // The PlayReadyLicenseTemplate class represents a license template 
                // for creating PlayReady licenses
                // to be returned to the end users. 
                // It contains the data on the content key in the license 
                // and any rights or restrictions to be 
                // enforced by the PlayReady DRM runtime when using the content key.
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                // Configure whether the license is persistent 
                // (saved in persistent storage on the client) 
                // or non-persistent (only held in memory while the player is using the license).  
                licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

                // AllowTestDevices controls whether test devices can use the license or not.  
                // If true, the MinimumSecurityLevel property of the license
                // is set to 150.  If false (the default), 
                // the MinimumSecurityLevel property of the license is set to 2000.
                licenseTemplate.AllowTestDevices = true;

                // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                // It grants the user the ability to playback the content subject to the zero or more restrictions 
                // configured in the license and on the PlayRight itself (for playback specific policy). 
                // Much of the policy on the PlayRight has to do with output restrictions 
                // which control the types of outputs that the content can be played over and 
                // any restrictions that must be put in place when using a given output.
                // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                //then the DRM runtime will only allow the video to be displayed over digital outputs 
                //(analog video outputs won’t be allowed to pass the content).

                // IMPORTANT: These types of restrictions can be very powerful 
                // but can also affect the consumer experience. 
                // If the output protections are configured too restrictive, 
                // the content might be unplayable on some clients. 
                // For more information, see the PlayReady Compliance Rules document.

                // For example:
                //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }


            private static string ConfigureWidevineLicenseTemplate()
            {
                var template = new WidevineMessage
                {
                    allowed_track_types = AllowedTrackTypes.SD_HD,
                    content_key_specs = new[]
                    {
                            new ContentKeySpecs
                            {
                                required_output_protection =
                                    new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                    policy_overrides = new
                    {
                        can_play = true,
                        can_persist = true,
                        can_renew = false
                    }
                };

                string configuration = JsonConvert.SerializeObject(template);
                return configuration;
            }


            static public IContentKey CreateCommonTypeContentKey()
            {
                // Create envelope encryption content key
                Guid keyId = Guid.NewGuid();
                byte[] contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }
        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="be375-133">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="be375-133">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="be375-134">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="be375-134">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="be375-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="be375-135">See also</span></span>
[<span data-ttu-id="be375-136">Použití běžného šifrování PlayReady nebo Widevine dynamický</span><span class="sxs-lookup"><span data-stu-id="be375-136">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="be375-137">Použití dynamické šifrování AES-128 a doručení klíče služby</span><span class="sxs-lookup"><span data-stu-id="be375-137">Using AES-128 Dynamic Encryption and Key Delivery Service</span></span>](media-services-protect-with-aes128.md)

[<span data-ttu-id="be375-138">Využití služeb partnerů k distribuci licencí Widevine pro Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="be375-138">Using partners to deliver Widevine licenses to Azure Media Services</span></span>](media-services-licenses-partner-integration.md)

