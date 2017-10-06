---
title: aaaProtect HLS obsahu se Microsoft PlayReady nebo Apple FairPlay - Azure | Microsoft Docs
description: "Toto téma poskytuje přehled a ukazuje, jak Azure Media Services toodynamically toouse šifrování obsahu HTTP Live Streaming (HLS) s Apple FairPlay. Také ukazuje, jak toouse hello Media Services licence doručení služby toodeliver tooclients licence FairPlay."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 91ca451e3e7bf0da1d74dac4c99180f08f39e4ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>Chránit váš obsah s Apple FairPlay nebo Microsoft PlayReady HLS
Azure Media Services umožňuje toodynamically můžete šifrovat obsah HTTP Live Streaming (HLS) pomocí hello následujících formátů:  

* **Nezašifrovaný klíč AES-128 obálky**

    Hello celý blok se šifrují pomocí hello **AES-128 CBC** režimu. iOS a OS X player je nativně podporované Hello dešifrování hello datového proudu. Další informace najdete v tématu [dynamického šifrování pomocí standardu AES-128 a doručení klíče služby](media-services-protect-with-aes128.md).
* **Apple FairPlay**

    Hello jednotlivých videa a audia ukázky jsou šifrována pomocí hello **AES-128 CBC** režimu. **Streamování FairPlay** (FPS) je integrována do hello, operačních systémů zařízení s nativní podporou na iOS a Apple TV. Prohlížeč Safari na OS X umožňuje FPS s využitím podpory rozhraní hello šifrované rozšíření média (EME).
* **Microsoft PlayReady**

Hello následující obrázek ukazuje hello **HLS + FairPlay nebo PlayReady dynamického šifrování** pracovního postupu.

![Diagram pracovního postupu dynamického šifrování](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Toto téma ukazuje, jak toouse Media Services toodynamically šifrovat obsah HLS s Apple FairPlay. Také ukazuje, jak toouse hello Media Services licence doručení služby toodeliver tooclients licence FairPlay.

> [!NOTE]
> Pokud také chcete tooencrypt vaše HLS obsahu pomocí technologie PlayReady, třeba toocreate běžné klíč obsahu a přidružte ji k asset. Je také nutné tooconfigure hello obsahu zásad autorizace klíče, jak je popsáno v [běžného dynamického šifrování PlayReady pomocí](media-services-protect-with-drm.md).
>
>

## <a name="requirements-and-considerations"></a>Požadavky a důležité informace

Následující Hello jsou požadovány při toodeliver Media Services, HLS zašifrován pomocí FairPlay a licence FairPlay toodeliver:

  * Účet Azure. Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
  * Účet Media Services. toocreate, najdete v části [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).
  * Zaregistrovat [vývoj programem](https://developer.apple.com/).
  * Apple vyžaduje hello vlastníka obsahu tooobtain hello [balíček pro nasazení](https://developer.apple.com/contact/fps/). Stav už implementované klíč zabezpečení modulu (KSM) pomocí služby Media Services a že žádáte hello konečné FPS balíčku. Jsou pokyny v hello konečné FPS balíček toogenerate certifikační a získat hello aplikace tajný klíč (požádejte). Můžete použít požádejte tooconfigure FairPlay.
  * Azure Media Services .NET SDK verze **3.6.0** nebo novější.

na straně doručení klíče služby Media Services musí být nastavena Hello následující věci:

  * **Aplikace Cert (AC)**: Toto je soubor .pfx, který obsahuje privátní klíč hello. Tento soubor vytvořte a šifrování pomocí hesla.

       Když konfigurujete zásady doručení klíče, je nutné zadat Tento soubor .pfx heslo a hello ve formátu Base64.

      Hello následující kroky popisují, jak souboru FairPlay toogenerate certifikátu .pfx:

    1. Nainstalujte OpenSSL z https://slproweb.com/products/Win32OpenSSL.html.

        Přejděte toohello složku, kde jsou hello FairPlay certifikátu a další soubory dodané společností Apple.
    2. Spusťte následující příkaz z příkazového řádku hello hello. Převede tento soubor .pem tooa soubor .cer hello.

        "C:\OpenSSL-Win32\bin\openssl.exe" x509-informovat der – v fairplay.cer-out fairplay out.pem
    3. Spusťte následující příkaz z příkazového řádku hello hello. Tento soubor .pfx tooa soubor .pem hello převede s privátním klíčem hello. Hello heslo pro soubor .pfx hello pak požaduje od OpenSSL.

        "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-export - out fairplay out.pfx-inkey privatekey.pem-v - passin file:privatekey-pem-pass.txt fairplay out.pem
  * **Heslo aplikace Cert**: hello heslo pro vytvoření souboru .pfx hello.
  * **ID aplikace Cert heslo**: je potřeba nahrát hello heslo, podobně jako toohow odesílají jiných klíčů Media Services. Použití hello **ContentKeyType.FairPlayPfxPassword** hello tooget hodnotu výčtu ID Media Services. Toto je požadované toouse uvnitř možnost zásady doručení klíče hello.
  * **IV**: je to hodnota náhodných 16 bajtů. Musí se shodovat hello iv v hello zásady doručení assetu. Vygenerování hello iv a umístí jej na obou místech: zásady doručení assetu hello a možnost zásady doručení klíče hello.
  * **Požádejte**: Tento klíč je oznámených při generování hello certifikační pomocí portálu pro vývojáře Apple hello. Každý vývojový tým obdrží jedinečný požádejte. Uložte kopii hello požádejte a uložit na bezpečném místě. Budete potřebovat tooconfigure požádejte jako FairPlayAsk tooMedia služby později.
  * **Požádejte ID**: Toto ID je získali při nahrávání požádejte ve službě Media Services. Požádejte musíte nahrát pomocí hello **ContentKeyType.FairPlayAsk** hodnota výčtu. Jako výsledek hello je vrácen hello Media Services ID a je to, co by měl být použit při nastavení možnost zásady doručení klíče hello.

Hello následujících akcí musí nastavit hello FPS na straně klienta:

  * **Aplikace Cert (AC)**: Toto je.cer/.der soubor, který obsahuje veřejný klíč hello, jaký operační systém hello používá tooencrypt některé datové části. Media Services musí tooknow o něm, protože ji požaduje hello přehrávač. Hello doručení klíče služby dešifruje ji pomocí hello odpovídající privátní klíč.

tooplay zpět FairPlay šifrovaného datového proudu, získat skutečné požádejte první a pak vygenerovat skutečné certifikát. Tento proces vytvoří všech tří částí:

  * souboru .der
  * soubor .pfx
  * hesla pro hello .pfx

Hello následující klienti podporují HLS s **AES-128 CBC** šifrování: Safari v systému OS X, Apple TV, iOS.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>Konfigurace FairPlay dynamické šifrování a licencí služeb doručování
Hello následují obecné kroky pro své assety chráníte pomocí FairPlay pomocí službu doručování licencí Media Services hello a také používáte dynamické šifrování.

1. Vytvoření assetu a nahrání souborů do hello asset.
2. Zakódujte asset hello, který obsahuje hello souboru toohello s adaptivní přenosovou rychlostí sady souborů MP4.
3. Vytvoření klíče k obsahu a přidružte ji k hello kódovaný asset.  
4. Nakonfigurujte zásady autorizace hello klíč obsahu. Zadejte hello následující:

   * Hello metodu doručení (v tomto případě FairPlay).
   * Konfigurace možností FairPlay zásad. Podrobnosti o tom tooconfigure FairPlay, najdete v části hello **ConfigureFairPlayPolicyOptions()** metoda v ukázce hello níže.

     > [!NOTE]
     > Obvykle budete by chtít tooconfigure FairPlay zásady Možnosti jenom jednou, protože budou mít pouze jednu sadu certifikace a ASK.
     >
     >
   * Omezení (otevřené nebo s tokenem).
   * Informace o konkrétní toohello doručení klíče typ, který definuje, jak je hello klíč doručen toohello klienta.
5. Konfigurace zásad doručení assetu hello. Konfigurace zásad doručení Hello zahrnuje:

   * Hello doručovací protokol (HLS).
   * Hello typ dynamického šifrování (běžné CBC šifrování).
   * adresu URL licence pořízení Hello.

     > [!NOTE]
     > Pokud chcete, aby toodeliver datový proud, který je šifrován FairPlay a jinému systému pro správu digitálních práv (DRM), je nutné tooconfigure samostatné doručení zásady:
     >
     > * Jeden IAssetDeliveryPolicy tooconfigure dynamické adaptivní datové proudy přes protokol HTTP (pomlčka) s běžné šifrování (CENC) (PlayReady a Widevine) a protokol Smooth s technologií PlayReady
     > * Jiné IAssetDeliveryPolicy tooconfigure FairPlay pro HLS
     >
     >
6. Vytvořte adresu URL pro streamování tooget Lokátor OnDemand.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>Použít doručení klíče FairPlay player aplikace
Přehrávač aplikace můžete vyvíjet pomocí hello iOS SDK. možnost tooplay toobe FairPlay obsahu, máte tooimplement hello licenci exchange protokolu. Tato položka protocol není zadán společností Apple. Tooeach aplikaci je, jak doručení klíče toosend požadavky. Hello doručení klíče služby Media Services FairPlay očekává hello SPC toocome jako zprávu kódovaného post www-form-url hello následující formulář:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Azure Media Player nepodporuje přehrávání FairPlay předinstalované hello. přehrávání FairPlay tooget na MAC OS X, získat hello ukázka player z hello účet pro vývojáře Apple.
>
>

## <a name="streaming-urls"></a>Adresy URL streamování
Pokud váš asset byla zašifrována pomocí více než jeden DRM, by měli používat značky šifrování v hello adresu URL streamování: (formát = 'm3u8-aapl' šifrování = 'xxx').

použít Hello následující aspekty:

* Lze zadat pouze nula nebo jeden typ šifrování.
* typ šifrování Hello nemá toobe zadaný v adrese URL hello, pokud jenom jeden šifrování byl použité toohello asset.
* typ šifrování Hello je malá a velká písmena.
* určit lze následující typy šifrování Hello:  
  * **šifrování cenc**: Common encryption (PlayReady nebo Widevine)
  * **cbcs-aapl**: FairPlay
  * **CBC**: šifrování AES obálky

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

1. Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 
2. Přidejte následující prvky příliš hello**appSettings** definované v souboru app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Příklad

Hello následující ukázka ukazuje obsah šifrován FairPlay hello možnost toouse toodeliver Media Services. Tato funkce byla zavedená v hello Azure Media Services SDK pro .NET verze 3.6.0. 

Přepište hello kód v souboru Program.cs kódem hello uvedené v této sekci.

>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady). Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

Ujistěte se že proměnné tooupdate toopoint toofolders kde jsou umístěné vaše vstupní soubory.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
    {
        class Program
        {
        // Read values from hello App.config file.
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

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }


        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Open",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                        Requirements = null
                    }
                    };


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Token Authorization Policy",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                        Requirements = tokenTemplateString,
                    }
                    };

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with hello cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound tooa real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key toogenerate hello response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating hello .pfx file.
            string pfxPassword = "<customer password for creating hello .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key toogenerate hello response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match hello iv in hello asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify hello .pfx file created by hello customer.
            var appCert = new X509Certificate2("path toohello .pfx file created by hello customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get hello FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // hello reason hello below code replaces "https://" with "skd://" is because
            // in hello IOS player sample code which you obtained in Apple developer account,
            // hello player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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


## <a name="next-steps-media-services-learning-paths"></a>Další kroky: Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
