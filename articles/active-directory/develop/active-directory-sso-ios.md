---
title: "aaaHow tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL | Microsoft Docs"
description: "Jak funkce hello toouse hello ADAL SDK tooenable jednotné přihlašování v aplikacích. "
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a><span data-ttu-id="9e1c7-103">Jak tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="9e1c7-103">How tooenable cross-app SSO on iOS using ADAL</span></span>
<span data-ttu-id="9e1c7-104">Poskytuje jednotné přihlašování (SSO), aby uživatelé pouze potřebovat tooenter jejich přihlašovací údaje jednou a mají tyto přihlašovací údaje automaticky pracovní aplikace je nyní očekávanou zákazníků.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-104">Providing Single Sign-On (SSO) so that users only need tooenter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="9e1c7-105">Hello potíže při zadání uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má toodo to více než jednou pro svůj produkt.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-105">hello difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has toodo this more than one time for your product.</span></span>

<span data-ttu-id="9e1c7-106">Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávat, že tyto přihlašovací údaje toobe dostupné toouse na všechny svoje aplikace bez ohledu na dodavatele hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials toobe available toouse across all their applications no matter hello vendor.</span></span>

<span data-ttu-id="9e1c7-107">Hello platformy Microsoft Identity, společně s naše sady SDK Microsoft Identity funguje všechny tento pevný a poskytuje hello možnost toodelight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sadu aplikací, nebo jako s naše schopností zprostředkovatele a Authenticator aplikacemi, v rámci celého zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-107">hello Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you hello ability toodelight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across hello entire device.</span></span>

<span data-ttu-id="9e1c7-108">Tento návod vám oznámí, jak tooconfigure naše sady SDK v rámci vaší aplikace tooprovide této výhody tooyour zákazníků.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-108">This walkthrough will tell you how tooconfigure our SDK within your application tooprovide this benefit tooyour customers.</span></span>

<span data-ttu-id="9e1c7-109">Tento postup platí pro:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="9e1c7-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e1c7-110">Azure Active Directory</span></span>
* <span data-ttu-id="9e1c7-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="9e1c7-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="9e1c7-112">Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="9e1c7-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="9e1c7-113">Podmíněný přístup k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e1c7-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="9e1c7-114">předchozí Hello dokumentu se předpokládá, víte, jak příliš[zřídit aplikace hello starší verze portálu pro Azure Active Directory](active-directory-how-to-integrate.md) a integraci aplikace s hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span><span class="sxs-lookup"><span data-stu-id="9e1c7-114">hello document preceding assumes you know how too[provision applications in hello legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span></span>

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a><span data-ttu-id="9e1c7-115">Koncepty jednotné přihlašování v hello Microsoft Identity Platform</span><span class="sxs-lookup"><span data-stu-id="9e1c7-115">SSO Concepts in hello Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="9e1c7-116">Zprostředkovatelé Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="9e1c7-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="9e1c7-117">Microsoft poskytuje aplikace pro každou platformu mobilních, které umožňují hello přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud toovalidate přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-117">Microsoft provides applications for every mobile platform that allow for hello bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where toovalidate credentials.</span></span> <span data-ttu-id="9e1c7-118">Tyto říkáme **makléřům**.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-118">We call these **brokers**.</span></span> <span data-ttu-id="9e1c7-119">Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete poslat toohello zařízení ve společnosti, který spravuje některé nebo všechny hello zařízení pro své zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-119">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages some or all of hello device for their employee.</span></span> <span data-ttu-id="9e1c7-120">Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo hello zařízení podle potřeby co správci IT.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-120">These brokers support managing security just for some applications or hello entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="9e1c7-121">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému toohello, technicky říká hello zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-121">In Windows, this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>

<span data-ttu-id="9e1c7-122">Další informace o tom, použijeme tyto zprostředkovatelé a jak vaši zákazníci mohou zobrazit je v jejich toku přihlášení pro platformu Microsoft Identity hello číst na.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-122">For more information on how we use these brokers and how your customers might see them in their login flow for hello Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="9e1c7-123">Vzory pro přihlašování na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="9e1c7-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="9e1c7-124">Toocredentials přístup na zařízení podle dva základní vzory pro platformu Microsoft Identity hello:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-124">Access toocredentials on devices follow two basic patterns for hello Microsoft Identity platform:</span></span>

* <span data-ttu-id="9e1c7-125">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9e1c7-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="9e1c7-126">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="9e1c7-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="9e1c7-127">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9e1c7-127">Non-broker assisted logins</span></span>
<span data-ttu-id="9e1c7-128">Přihlášení odbornou non-broker jsou přihlášení prostředí, které dojít vložené s hello aplikací a použít místní úložiště hello na hello zařízení pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-128">Non-broker assisted logins are login experiences that happen inline with hello application and use hello local storage on hello device for that application.</span></span> <span data-ttu-id="9e1c7-129">Toto úložiště může být sdíleny s aplikací, ale hello přihlašovací údaje jsou úzce vázané toohello aplikace nebo sadu aplikací pomocí tohoto pověření.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-129">This storage may be shared across applications but hello credentials are tightly bound toohello app or suite of apps using that credential.</span></span> <span data-ttu-id="9e1c7-130">Jste s největší pravděpodobností došlo k to v mnoha mobilních aplikacích. když zadáte uživatelské jméno a heslo v rámci vlastní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-130">You've most likely experienced this in many mobile applications when you enter a username and password within hello application itself.</span></span>

<span data-ttu-id="9e1c7-131">Tyto přihlášení mít hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-131">These logins have hello following benefits:</span></span>

* <span data-ttu-id="9e1c7-132">Činnost koncového uživatele existuje zcela v rámci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-132">User experience exists entirely within hello application.</span></span>
* <span data-ttu-id="9e1c7-133">Přihlašovací údaje můžete sdílet mezi aplikací, které jsou podepsány hello stejný certifikát, poskytuje prostředí přihlašování tooyour sady aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-133">Credentials can be shared across applications that are signed by hello same certificate, providing a single sign-on experience tooyour suite of applications.</span></span>
* <span data-ttu-id="9e1c7-134">Ovládací prvek kolem hello prostředí přihlášení je k dispozici toohello aplikace před a po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-134">Control around hello experience of logging in is provided toohello application before and after sign-in.</span></span>

<span data-ttu-id="9e1c7-135">Tyto přihlášení mít hello následující nevýhody:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-135">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="9e1c7-136">Uživatele nelze jednotného přihlašování napříč všechny aplikace, které používají Microsoft Identity pouze v rámci těchto Identities Microsoft, které má vaše aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="9e1c7-137">Aplikaci nelze použít s pokročilejší obchodní funkce jako je například podmíněný přístup nebo použití hello InTune sadu produktů.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-137">Your application cannot be used with more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="9e1c7-138">Aplikace nepodporuje ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="9e1c7-139">Zde je reprezentace jak hello Microsoft Identity sady SDK pracovat s hello sdílené úložiště vaše aplikace tooenable jednotné přihlašování:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-139">Here is a representation of how hello Microsoft Identity SDKs work with hello shared storage of your applications tooenable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="9e1c7-140">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="9e1c7-140">Broker assisted logins</span></span>
<span data-ttu-id="9e1c7-141">S pomocí zprostředkovatele přihlášení jsou přihlášení prostředí, v rámci aplikace hello zprostředkovatele, které používají úložiště hello a zabezpečení hello zprostředkovatele tooshare přihlašovací údaje ve všech aplikacích na hello zařízení, které se vztahují hello platformy Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-141">Broker-assisted logins are login experiences that occur within hello broker application and use hello storage and security of hello broker tooshare credentials across all applications on hello device that apply hello Microsoft Identity platform.</span></span> <span data-ttu-id="9e1c7-142">To znamená, že vaše aplikace závisí na hello zprostředkovatele toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-142">This means that your applications rely on hello broker toosign users in.</span></span> <span data-ttu-id="9e1c7-143">Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete poslat toohello zařízení ve společnosti, který spravuje hello zařízení pro svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages hello device for their user.</span></span> <span data-ttu-id="9e1c7-144">Příkladem tento typ aplikace je aplikace Microsoft Authenticator hello v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-144">An example of this type of application is hello Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="9e1c7-145">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému toohello, technicky říká hello zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-145">In Windows this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>
<span data-ttu-id="9e1c7-146">Hello prostředí se liší podle platformy a v některých případech může být rušivý toousers není správně spravovat.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-146">hello experience varies by platform and can sometimes be disruptive toousers if not managed correctly.</span></span> <span data-ttu-id="9e1c7-147">Jste pravděpodobně nejvíce obeznámeni s tento vzor, pokud máte nainstalovanou aplikací hello Facebook a použijete Facebook připojit z jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-147">You're probably most familiar with this pattern if you have hello Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="9e1c7-148">Hello používá platformy Microsoft Identity hello stejného vzoru.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-148">hello Microsoft Identity platform uses hello same pattern.</span></span>

<span data-ttu-id="9e1c7-149">Pro iOS důsledkem tooa "přechodu" animace kam aplikace je odeslána toohello pozadí při aplikace Microsoft Authenticator hello dodává popředí toohello pro uživatele tooselect hello účtu, který mu toosign s.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-149">For iOS this leads tooa "transition" animation where your application is sent toohello background while hello Microsoft Authenticator applications comes toohello foreground for hello user tooselect which account they would like toosign in with.</span></span>  

<span data-ttu-id="9e1c7-150">Pro Android a Windows hello účet výběru, zobrazí se na aplikace, což je méně rušivý toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-150">For Android and Windows hello account chooser is displayed on top of your application which is less disruptive toohello user.</span></span>

#### <a name="how-hello-broker-gets-invoked"></a><span data-ttu-id="9e1c7-151">Jak získá vyvolána hello zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9e1c7-151">How hello broker gets invoked</span></span>
<span data-ttu-id="9e1c7-152">Pokud je v hello zařízení, jako jsou hello Microsoft Authenticator aplikace hello SDK služby Microsoft Identity bude automaticky nainstalován kompatibilní zprostředkovatele hello pracovní vyvolání hello zprostředkovatele pro vás, když uživatel označuje, že si přejí toolog pomocí libovolného účtu z Platforma Microsoft Identity Hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-152">If a compatible broker is installed on hello device, like hello Microsoft Authenticator application, hello Microsoft Identity SDKs will automatically do hello work of invoking hello broker for you when a user indicates they wish toolog in using any account from hello Microsoft Identity platform.</span></span> <span data-ttu-id="9e1c7-153">Tento účet může být osobní Account Microsoft, pracovní nebo školní účet, nebo účtu, který zadáte a hostitele v Azure pomocí našich produktů B2C a B2B.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 

 #### <a name="how-we-ensure-hello-application-is-valid"></a><span data-ttu-id="9e1c7-154">Jak jsme zkontrolujte aplikace hello je platný</span><span class="sxs-lookup"><span data-stu-id="9e1c7-154">How we ensure hello application is valid</span></span>
 
 <span data-ttu-id="9e1c7-155">Hello nutné tooensure hello identita broker hello volání aplikace je zásadní toohello zabezpečení, které poskytujeme v s pomocí zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-155">hello need tooensure hello identity of an application call hello broker is crucial toohello security we provide in broker assisted logins.</span></span> <span data-ttu-id="9e1c7-156">IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny hello určená pro aplikaci legitimní hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive hello tokens meant for hello legitimate application.</span></span> <span data-ttu-id="9e1c7-157">tooensure, které jsme vždy komunikují s hello správné aplikace za běhu, požádáme hello vývojáře tooprovide vlastní redirectURI při registraci své aplikace se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-157">tooensure we are always communicating with hello right application at runtime, we ask hello developer tooprovide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="9e1c7-158">**Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.**</span><span class="sxs-lookup"><span data-stu-id="9e1c7-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="9e1c7-159">Tato vlastní redirectURI obsahuje hello ID sady prostředků aplikace hello a je zajištěna toobe jedinečný toohello aplikace hello Apple App Store.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-159">This custom redirectURI contains hello Bundle ID of hello application and is ensured toobe unique toohello application by hello Apple App Store.</span></span> <span data-ttu-id="9e1c7-160">Když se aplikace volání hello broker, hello broker zeptá hello iOS operačního systému tooprovide hello její ID sady, který volá zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-160">When an application calls hello broker, hello broker asks hello iOS operating system tooprovide it with hello Bundle ID that called hello broker.</span></span> <span data-ttu-id="9e1c7-161">Hello zprostředkovatele poskytuje toto ID sady tooMicrosoft systému identity tooour volání hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-161">hello broker provides this Bundle ID tooMicrosoft in hello call tooour identity system.</span></span> <span data-ttu-id="9e1c7-162">Pokud hello ID sady prostředků aplikace hello neodpovídá hello poskytnuté ID sady toous vývojářem hello během registrace, jsme odepře přístup toohello tokeny pro hello prostředků hello aplikace požaduje.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-162">If hello Bundle ID of hello application does not match hello Bundle ID provided toous by hello developer during registration, we will deny access toohello tokens for hello resource hello application is requesting.</span></span> <span data-ttu-id="9e1c7-163">Tato kontrola zajistí, že pouze hello aplikace registrovaných hello vývojáře přijímá tokeny.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-163">This check ensures that only hello application registered by hello developer receives tokens.</span></span>

<span data-ttu-id="9e1c7-164">**Hello vývojáře má hello volbu, pokud hello Microsoft Identity SDK volá zprostředkovatele hello nebo používá odbornou toku hello bez zprostředkovatele.**</span><span class="sxs-lookup"><span data-stu-id="9e1c7-164">**hello developer has hello choice of if hello Microsoft Identity SDK calls hello broker or uses hello non-broker assisted flow.**</span></span> <span data-ttu-id="9e1c7-165">Ale pokud hello vývojáře vybere není toouse hello s asistencí služby broker toku nich došlo ke ztrátě výhodou hello pomocí jednotného přihlašování k přihlašovacích údajů tohoto uživatele hello může mít již přidán do zařízení hello a zabrání jejich aplikaci z používá s funkcemi obchodní Microsoft nabízí svým zákazníkům, jako je například podmíněný přístup, možnosti správy Intune a ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-165">However if hello developer chooses not toouse hello broker-assisted flow they lose hello benefit of using SSO credentials that hello user may have already added on hello device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="9e1c7-166">Tyto přihlášení mít hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-166">These logins have hello following benefits:</span></span>

* <span data-ttu-id="9e1c7-167">Uživatel narazí na všechny svoje aplikace bez ohledu na dodavatele hello jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-167">User experiences SSO across all their applications no matter hello vendor.</span></span>
* <span data-ttu-id="9e1c7-168">Aplikace můžete použít dalších pokročilých funkcí firmy, jako je například podmíněný přístup nebo pomocí sady InTune hello produktů.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-168">Your application can use more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="9e1c7-169">Aplikace může podporovat ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="9e1c7-170">Mnohem bezpečnější přihlašování hello identity aplikace hello a hello uživatele jsou ověřené aplikací hello zprostředkovatele s další bezpečnostní algoritmů hash a šifrování.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-170">Much more secure sign-in experience as hello identity of hello application and hello user are verified by hello broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="9e1c7-171">Tyto přihlášení mít hello následující nevýhody:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-171">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="9e1c7-172">V iOS převedena hello uživatele mimo prostředí vaší aplikace, když je možné zvolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-172">In iOS hello user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="9e1c7-173">Ztrátě hello možnost toomanage hello přihlašovací prostředí pro vaši zákazníci v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-173">Loss of hello ability toomanage hello login experience for your customers within your application.</span></span>

<span data-ttu-id="9e1c7-174">Zde je reprezentace jak pracují SDK služby Microsoft Identity hello se hello zprostředkovatel aplikace tooenable jednotné přihlašování:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-174">Here is a representation of how hello Microsoft Identity SDKs work with hello broker applications tooenable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

<span data-ttu-id="9e1c7-175">Díky této základní informace musí být schopný toobetter pochopit a implementovat jednotné přihlašování v rámci vaší aplikace pomocí platformy Microsoft Identity hello a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-175">Armed with this background information you should be able toobetter understand and implement SSO within your application using hello Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="9e1c7-176">Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="9e1c7-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="9e1c7-177">Tady používáme hello ADAL iOS SDK na:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-177">Here we use hello ADAL iOS SDK to:</span></span>

* <span data-ttu-id="9e1c7-178">Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací</span><span class="sxs-lookup"><span data-stu-id="9e1c7-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="9e1c7-179">Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9e1c7-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="9e1c7-180">Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e1c7-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="9e1c7-181">Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat hello SDK služby Microsoft Identity mnohem složitější hello přihlašování pro vás.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-181">For non-broker assisted SSO across applications hello Microsoft Identity SDKs manage much of hello complexity of SSO for you.</span></span> <span data-ttu-id="9e1c7-182">To zahrnuje hello správné uživatelské hledání v mezipaměti hello a udržování seznam přihlášeného uživatele pro tooquery je.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-182">This includes finding hello right user in hello cache and maintaining a list of logged in users for you tooquery.</span></span>

<span data-ttu-id="9e1c7-183">tooenable jednotného přihlašování napříč aplikacemi, které vlastníte, které že budete potřebovat toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-183">tooenable SSO across applications you own you need toodo hello following:</span></span>

1. <span data-ttu-id="9e1c7-184">Ujistěte se, všechna vaše aplikace uživatele hello stejné ID klienta, nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-184">Ensure all your applications user hello same Client ID or Application ID.</span></span>
2. <span data-ttu-id="9e1c7-185">Zajistěte, aby všechny vaše aplikace hello sdílené složky stejný podpisový certifikát od společnosti Apple, tak, že můžete sdílet řetězce klíčů</span><span class="sxs-lookup"><span data-stu-id="9e1c7-185">Ensure that all of your applications share hello same signing certificate from Apple so that you can share keychains</span></span>
3. <span data-ttu-id="9e1c7-186">Žádosti o hello stejné nárocích řetězce klíčů pro každou z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-186">Request hello same keychain entitlement for each of your applications.</span></span>
4. <span data-ttu-id="9e1c7-187">Řekněte hello Microsoft Identity sady SDK o řetězce hello sdílených klíčů, že nám chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-187">Tell hello Microsoft Identity SDKs about hello shared keychain you want us toouse.</span></span>

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a><span data-ttu-id="9e1c7-188">Pomocí hello stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9e1c7-188">Using hello same Client ID / Application ID for all hello applications in your suite of apps</span></span>
<span data-ttu-id="9e1c7-189">Aby tooknow platformy Microsoft Identity hello, že je povolená tooshare tokeny mezi aplikací každý z vašich aplikací bude nutné tooshare hello stejné ID klienta, nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-189">In order for hello Microsoft Identity platform tooknow that it's allowed tooshare tokens across your applications, each of your applications will need tooshare hello same Client ID or Application ID.</span></span> <span data-ttu-id="9e1c7-190">Toto je hello jedinečný identifikátor, který byl poskytnut tooyou při registraci vaší první aplikace hello portálu.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-190">This is hello unique identifier that was provided tooyou when you registered your first application in hello portal.</span></span>

<span data-ttu-id="9e1c7-191">Asi vás zajímá, jak bude identifikujete různé aplikace hello toohello služba Microsoft Identity, pokud používá stejné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="9e1c7-191">You may be wondering how you will identify different apps toohello Microsoft Identity service if it uses hello same Application ID.</span></span> <span data-ttu-id="9e1c7-192">je Hello odpověď s hello **identifikátory URI přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-192">hello answer is with hello **Redirect URIs**.</span></span> <span data-ttu-id="9e1c7-193">Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-193">Each application can have multiple Redirect URIs registered in hello onboarding portal.</span></span> <span data-ttu-id="9e1c7-194">Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-194">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="9e1c7-195">Zde je příklad, jak to vypadá:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-195">An example of how this looks is below:</span></span>

<span data-ttu-id="9e1c7-196">Identifikátor URI přesměrování app1:`x-msauth-mytestiosapp://com.myapp.mytestapp`</span><span class="sxs-lookup"><span data-stu-id="9e1c7-196">App1 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`</span></span>

<span data-ttu-id="9e1c7-197">Identifikátor URI přesměrování počítači App2:`x-msauth-mytestiosapp://com.myapp.mytestapp2`</span><span class="sxs-lookup"><span data-stu-id="9e1c7-197">App2 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`</span></span>

<span data-ttu-id="9e1c7-198">Identifikátor URI přesměrování App3:`x-msauth-mytestiosapp://com.myapp.mytestapp3`</span><span class="sxs-lookup"><span data-stu-id="9e1c7-198">App3 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`</span></span>

<span data-ttu-id="9e1c7-199">....</span><span class="sxs-lookup"><span data-stu-id="9e1c7-199">....</span></span>

<span data-ttu-id="9e1c7-200">Tyto jsou vnořeny pod stejným ID klienta hello / ID aplikace a vyhledávat na základě hello redirect URI vrátíte toous v konfiguraci sady SDK.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-200">These are nested under hello same client ID / application ID and looked up based on hello redirect URI you return toous in your SDK configuration.</span></span>

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


<span data-ttu-id="9e1c7-201">*Všimněte si, že hello formát tyto identifikátory URI přesměrování jsou vysvětleny níže. Může použít jakékoli identifikátor URI pro přesměrování, pokud chcete toosupport hello zprostředkovatele, v takovém případě se musí vypadat podobně jako výše hello*</span><span class="sxs-lookup"><span data-stu-id="9e1c7-201">*Note that hello format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish toosupport hello broker, in which case they must look something like hello above*</span></span>

#### <a name="create-keychain-sharing-between-applications"></a><span data-ttu-id="9e1c7-202">Vytvořte sdílení mezi aplikacemi řetězce klíčů</span><span class="sxs-lookup"><span data-stu-id="9e1c7-202">Create keychain sharing between applications</span></span>
<span data-ttu-id="9e1c7-203">Povolení sdílení řetězce klíčů je nad rámec tohoto dokumentu hello a jejich dokument nezabývá společností Apple [přidání schopností](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span><span class="sxs-lookup"><span data-stu-id="9e1c7-203">Enabling keychain sharing is beyond hello scope of this document and covered by Apple in their document [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span></span> <span data-ttu-id="9e1c7-204">Co je důležité se rozhodnout, jak vaše toobe řetězce klíčů, názvem a přidejte tuto funkci u všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-204">What is important is that you decide what you want your keychain toobe called and add that capability across all your applications.</span></span>

<span data-ttu-id="9e1c7-205">Pokud máte oprávnění nastavit správně jste měli vidět soubor v adresáři projektu s názvem `entitlements.plist` obsahující něco, co vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-205">When you do have entitlements set up correctly you should see a file in your project directory entitled `entitlements.plist` that contains something that looks like hello following:</span></span>

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

<span data-ttu-id="9e1c7-206">Jakmile máte nárok řetězce klíčů hello povolené v každé z vašich aplikací a jsou připravené toouse jednotné přihlašování, řekněte hello Microsoft Identity SDK o vaší řetězce klíčů pomocí hello následující nastavení v vaší `ADAuthenticationSettings` s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-206">Once you have hello keychain entitlement enabled in each of your applications, and you are ready toouse SSO, tell hello Microsoft Identity SDK about your keychain by using hello following setting in your `ADAuthenticationSettings` with hello following setting:</span></span>

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> <span data-ttu-id="9e1c7-207">Když sdílíte mezi vaší aplikace řetězce klíčů všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny hello napříč aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-207">When you share a keychain across your applications any application can delete users or worse delete all hello tokens across your application.</span></span> <span data-ttu-id="9e1c7-208">To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na práce pozadí toodo tokeny hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-208">This is particularly disastrous if you have applications that rely on hello tokens toodo background work.</span></span> <span data-ttu-id="9e1c7-209">Sdílení řetězce klíčů odebrat znamená, že musí být velmi opatrní při všech operací prostřednictvím sady SDK Identity Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-209">Sharing a keychain means that you must be very careful in any and all remove operations through hello Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="9e1c7-210">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="9e1c7-210">That's it!</span></span> <span data-ttu-id="9e1c7-211">Hello Microsoft Identity SDK bude nyní sdílet přihlašovací údaje ve všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-211">hello Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="9e1c7-212">seznam uživatelů Hello bude také sdílet mezi instancemi aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-212">hello user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="9e1c7-213">Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e1c7-213">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="9e1c7-214">Hello schopnost aplikaci toouse všechny zprostředkovatele, který je nainstalován na zařízení hello je **ve výchozím nastavení vypnuté**.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-214">hello ability for an application toouse any broker that is installed on hello device is **turned off by default**.</span></span> <span data-ttu-id="9e1c7-215">V pořadí toouse vaší aplikace pomocí zprostředkovatele hello musí provést některé další konfiguraci a přidejte některé aplikaci tooyour kódu.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-215">In order toouse your application with hello broker you must do some additional configuration and add some code tooyour application.</span></span>

<span data-ttu-id="9e1c7-216">toofollow Hello kroky jsou:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-216">hello steps toofollow are:</span></span>

1. <span data-ttu-id="9e1c7-217">Povolte režim zprostředkovatele v kódu aplikace volání toohello MS SDK.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-217">Enable broker mode in your application code's call toohello MS SDK.</span></span>
2. <span data-ttu-id="9e1c7-218">Vytvořit nový identifikátor URI přesměrování a zadejte aplikaci hello tooboth a registrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-218">Establish a new redirect URI and provide that tooboth hello app and your app registration.</span></span>
3. <span data-ttu-id="9e1c7-219">Probíhá registrace schéma adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-219">Registering a URL Scheme.</span></span>
4. <span data-ttu-id="9e1c7-220">Podpora iOS9: přidání souboru info.plist tooyour oprávnění.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-220">iOS9 Support: Add a permission tooyour info.plist file.</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="9e1c7-221">Krok 1: Povolení režimu zprostředkovatele v aplikaci</span><span class="sxs-lookup"><span data-stu-id="9e1c7-221">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="9e1c7-222">Hello možnost pro vaše aplikace toouse hello broker zapnutý, při vytváření kontextu"hello" nebo počáteční nastavení ověřování objektu.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-222">hello ability for your application toouse hello broker is turned on when you create hello "context" or initial setup of your Authentication object.</span></span> <span data-ttu-id="9e1c7-223">To provedete nastavením typ vaše přihlašovací údaje ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-223">You do this by setting your credentials type in your code:</span></span>

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
<span data-ttu-id="9e1c7-224">Hello `AD_CREDENTIALS_AUTO` nastavení povolí hello Microsoft Identity SDK tootry toocall out toohello zprostředkovatele `AD_CREDENTIALS_EMBEDDED` zabrání volání zprostředkovatele toohello hello Microsoft Identity SDK.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-224">hello `AD_CREDENTIALS_AUTO` setting will allow hello Microsoft Identity SDK tootry toocall out toohello broker, `AD_CREDENTIALS_EMBEDDED` will prevent hello Microsoft Identity SDK from calling toohello broker.</span></span>

#### <a name="step-2-registering-a-url-scheme"></a><span data-ttu-id="9e1c7-225">Krok 2: Registrace schéma adresy URL</span><span class="sxs-lookup"><span data-stu-id="9e1c7-225">Step 2: Registering a URL Scheme</span></span>
<span data-ttu-id="9e1c7-226">Platforma Microsoft Identity Hello používá zprostředkovatele hello tooinvoke adresy URL a pak návratový řízení back tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-226">hello Microsoft Identity platform uses URLs tooinvoke hello broker and then return control back tooyour application.</span></span> <span data-ttu-id="9e1c7-227">toofinish tuto dobu odezvy, je nutné schéma adresy URL zaregistrován pro vaši aplikaci této hello Microsoft Identity platformy bude vědět o.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-227">toofinish that round trip you need a URL scheme registered for your application that hello Microsoft Identity platform will know about.</span></span> <span data-ttu-id="9e1c7-228">To může být kromě tooany jiná schémata aplikací mít může dříve registrován u vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-228">This can be in addition tooany other app schemes you may have previously registered with your application.</span></span>

> [!WARNING]
> <span data-ttu-id="9e1c7-229">Doporučujeme nastavit adresu URL schéma poměrně jedinečný toominimize hello pravděpodobnost jiné aplikaci pomocí hello hello stejné schéma adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-229">We recommend making hello URL scheme fairly unique toominimize hello chances of another app using hello same URL scheme.</span></span> <span data-ttu-id="9e1c7-230">Apple nevynucuje hello jedinečnosti schémata URL, které jsou zaregistrovány v obchodě s aplikacemi hello.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-230">Apple does not enforce hello uniqueness of URL schemes that are registered in hello app store.</span></span>
> 
> 

<span data-ttu-id="9e1c7-231">Níže je příklad, jak se objeví v konfigurace projektu.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-231">Below is an example of how this appears in your project configuration.</span></span> <span data-ttu-id="9e1c7-232">Také to lze provést v prostředí XCode také:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-232">You may also do this in XCode as well:</span></span>

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="9e1c7-233">Krok 3: Stanovení na nový identifikátor URI s vaše schéma adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="9e1c7-233">Step 3: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="9e1c7-234">V pořadí tooensure, že se vždy vrací, že pověření hello tokeny toohello správnou aplikaci potřebujeme toomake se, že jsme zpětné volání, můžete ověřit tooyour aplikace tak, aby hello operačního systému iOS.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-234">In order tooensure that we always return hello credential tokens toohello correct application, we need toomake sure we call back tooyour application in a way that hello iOS operating system can verify.</span></span> <span data-ttu-id="9e1c7-235">Zprostředkovatel aplikací společnosti Microsoft sestavy toohello Hello iOS operačního systému hello ID sady prostředků aplikace hello volání ho.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-235">hello iOS operating system reports toohello Microsoft broker applications hello Bundle ID of hello application calling it.</span></span> <span data-ttu-id="9e1c7-236">To nelze maskování podvodný aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-236">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="9e1c7-237">Proto jsme využít to společně s hello URI naše tooensure aplikace zprostředkovatele že hello tokeny jsou vráceny toohello správnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-237">Therefore, we leverage this along with hello URI of our broker application tooensure that hello tokens are returned toohello correct application.</span></span> <span data-ttu-id="9e1c7-238">Je nutné, můžete tooestablish tento jedinečný identifikátor URI pro přesměrování jak v aplikaci a sadu jako identifikátor URI přesměrování v našem portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-238">We require you tooestablish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="9e1c7-239">Váš identifikátor URI pro přesměrování musí být ve formátu správné hello:</span><span class="sxs-lookup"><span data-stu-id="9e1c7-239">Your redirect URI must be in hello proper form of:</span></span>

`<app-scheme>://<your.bundle.id>`

<span data-ttu-id="9e1c7-240">například: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="9e1c7-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span></span>

<span data-ttu-id="9e1c7-241">Tento identifikátor URI pro přesměrování musí toobe zadaný v registraci vaší aplikace pomocí hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9e1c7-241">This Redirect URI needs toobe specified in your app registration using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="9e1c7-242">Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="9e1c7-242">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a><span data-ttu-id="9e1c7-243">Krok 3a: přidejte identifikátor URI pro přesměrování v aplikaci a vývojářů ověřování na základě certifikátů portálu toosupport</span><span class="sxs-lookup"><span data-stu-id="9e1c7-243">Step 3a: Add a redirect URI in your app and dev portal toosupport certificate based authentication</span></span>
<span data-ttu-id="9e1c7-244">ověřování na základě certifikátu toosupport druhý "msauth" musí toobe zaregistrovaný v aplikaci a hello [portál Azure](https://portal.azure.com/) toohandle ověření certifikátu, pokud chcete tooadd, které podporují ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-244">toosupport cert based authentication a second "msauth"  needs toobe registered in your application and hello [Azure portal](https://portal.azure.com/) toohandle certificate authentication if you wish tooadd that support in your application.</span></span>

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

<span data-ttu-id="9e1c7-245">například: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="9e1c7-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span></span>

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a><span data-ttu-id="9e1c7-246">Krok 4: iOS9: Přidat aplikaci tooyour parametr konfigurace</span><span class="sxs-lookup"><span data-stu-id="9e1c7-246">Step 4: iOS9: Add a configuration parameter tooyour app</span></span>
<span data-ttu-id="9e1c7-247">ADAL používá – canOpenURL: toocheck, pokud je v zařízení hello nainstalován hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-247">ADAL uses –canOpenURL: toocheck if hello broker is installed on hello device.</span></span> <span data-ttu-id="9e1c7-248">V systému iOS 9 Apple uzamčené co schémata aplikaci dotázat.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-248">In iOS 9 Apple locked down what schemes an application can query for.</span></span> <span data-ttu-id="9e1c7-249">Budete potřebovat tooadd "msauth" toohello LSApplicationQueriesSchemes část vaší `info.plist file`.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-249">You will need tooadd “msauth” toohello LSApplicationQueriesSchemes section of your `info.plist file`.</span></span>

<span data-ttu-id="9e1c7-250"><key>LSApplicationQueriesSchemes</key></span><span class="sxs-lookup"><span data-stu-id="9e1c7-250"><key>LSApplicationQueriesSchemes</key></span></span>

<span data-ttu-id="9e1c7-251"><array><string>msauth</string>
</array></span><span class="sxs-lookup"><span data-stu-id="9e1c7-251"><array> <string>msauth</string>
</array></span></span>

### <a name="youve-configured-sso"></a><span data-ttu-id="9e1c7-252">Jednotné přihlašování jste nakonfigurovali!</span><span class="sxs-lookup"><span data-stu-id="9e1c7-252">You've configured SSO!</span></span>
<span data-ttu-id="9e1c7-253">Nyní hello Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele hello, pokud je k dispozici na svém zařízení.</span><span class="sxs-lookup"><span data-stu-id="9e1c7-253">Now hello Microsoft Identity SDK will automatically both share credentials across your applications and invoke hello broker if it's present on their device.</span></span>

