---
title: "Postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL | Microsoft Docs"
description: "Jak používat funkce sady ADAL SDK povolit jednotné přihlašování v rámci vaší aplikace. "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 9c7e959530a836fe5ddf74708363a636c39b3cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a><span data-ttu-id="8a98d-103">Postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="8a98d-103">How to enable cross-app SSO on Android using ADAL</span></span>
<span data-ttu-id="8a98d-104">Pokud jednotné přihlašování (SSO), aby uživatelé stačí jednou zadat své přihlašovací údaje a mají tyto přihlašovací údaje automaticky fungovat na všech aplikací nyní očekává zákazníků.</span><span class="sxs-lookup"><span data-stu-id="8a98d-104">Providing Single Sign-On (SSO) so that users only need to enter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="8a98d-105">Problémy se zadáním uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má k tomu více než jednou pro svůj produkt.</span><span class="sxs-lookup"><span data-stu-id="8a98d-105">The difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has to do this more than one time for your product.</span></span>

<span data-ttu-id="8a98d-106">Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávají, že ty pověření být k dispozici pro použití na všechny svoje aplikace bez ohledu na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials to be available to use across all their applications no matter the vendor.</span></span>

<span data-ttu-id="8a98d-107">Do platformy Microsoft Identity, společně s naše sady SDK Identity Microsoft nepodporuje tento pevný pracovní pro vás a vám dává možnost delight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sada aplikací nebo, stejně jako u našich schopností zprostředkovatele a ověřovací aplikacemi, v rámci celého zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a98d-107">The Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you the ability to delight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across the entire device.</span></span>

<span data-ttu-id="8a98d-108">Tento návod popisuje postup konfigurace naše sady SDK v rámci aplikace umožní vašim zákazníkům poskytovat této výhody.</span><span class="sxs-lookup"><span data-stu-id="8a98d-108">This walkthrough will tell you how to configure our SDK within your application to provide this benefit to your customers.</span></span>

<span data-ttu-id="8a98d-109">Tento postup platí pro:</span><span class="sxs-lookup"><span data-stu-id="8a98d-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="8a98d-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a98d-110">Azure Active Directory</span></span>
* <span data-ttu-id="8a98d-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8a98d-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="8a98d-112">Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="8a98d-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="8a98d-113">Podmíněný přístup k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a98d-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="8a98d-114">Předchozí dokument předpokládá, že víte, jak [zřídit aplikace na portálu pro starší verze pro Azure Active Directory](active-directory-how-to-integrate.md) a integrované aplikace s [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span><span class="sxs-lookup"><span data-stu-id="8a98d-114">The document preceding assumes you know how to [provision applications in the legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with the [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span></span>

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a><span data-ttu-id="8a98d-115">Koncepty jednotné přihlašování v platformě Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="8a98d-115">SSO Concepts in the Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="8a98d-116">Zprostředkovatelé Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="8a98d-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="8a98d-117">Společnost Microsoft poskytuje aplikace pro každou platformu mobilních umožňujících přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud se ověřit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8a98d-117">Microsoft provides applications for every mobile platform that allow for the bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where to validate credentials.</span></span> <span data-ttu-id="8a98d-118">Tyto říkáme **makléřům**.</span><span class="sxs-lookup"><span data-stu-id="8a98d-118">We call these **brokers**.</span></span> <span data-ttu-id="8a98d-119">Na iOS a Android tyto položky jsou poskytovány prostřednictvím ke stažení aplikací, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje některých nebo všech zařízení pro své zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="8a98d-119">On iOS and Android these are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages some or all of the device for their employee.</span></span> <span data-ttu-id="8a98d-120">Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo ze zařízení podle potřeby co správci IT.</span><span class="sxs-lookup"><span data-stu-id="8a98d-120">These brokers support managing security just for some applications or the entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="8a98d-121">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="8a98d-121">In Windows, this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>

<span data-ttu-id="8a98d-122">Další informace o tom, použijeme tyto zprostředkovatelé a jak vaši zákazníci mohou zobrazit je v jejich toku přihlášení pro čtení na platformu Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="8a98d-122">For more information on how we use these brokers and how your customers might see them in their login flow for the Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="8a98d-123">Vzory pro přihlašování na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="8a98d-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="8a98d-124">Přístup k přihlašovacím údajům v zařízení podle dva základní vzory pro platformu Microsoft Identity:</span><span class="sxs-lookup"><span data-stu-id="8a98d-124">Access to credentials on devices follow two basic patterns for the Microsoft Identity platform:</span></span>

* <span data-ttu-id="8a98d-125">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="8a98d-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="8a98d-126">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="8a98d-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="8a98d-127">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="8a98d-127">Non-broker assisted logins</span></span>
<span data-ttu-id="8a98d-128">Přihlášení odbornou non-broker jsou možností přihlášení, které dojít vložené s aplikací a použít místní úložiště na zařízení pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a98d-128">Non-broker assisted logins are login experiences that happen inline with the application and use the local storage on the device for that application.</span></span> <span data-ttu-id="8a98d-129">Toto úložiště může být sdíleny s aplikací, ale přihlašovací údaje jsou úzce vázaný aplikace nebo sadu aplikací pomocí tohoto pověření.</span><span class="sxs-lookup"><span data-stu-id="8a98d-129">This storage may be shared across applications but the credentials are tightly bound to the app or suite of apps using that credential.</span></span> <span data-ttu-id="8a98d-130">Jste s největší pravděpodobností došlo k to v mnoha mobilních aplikacích. když zadáte uživatelské jméno a heslo v rámci vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-130">You've most likely experienced this in many mobile applications when you enter a username and password within the application itself.</span></span>

<span data-ttu-id="8a98d-131">Tyto přihlášení mají následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8a98d-131">These logins have the following benefits:</span></span>

* <span data-ttu-id="8a98d-132">Činnost koncového uživatele zcela v aplikaci existuje.</span><span class="sxs-lookup"><span data-stu-id="8a98d-132">User experience exists entirely within the application.</span></span>
* <span data-ttu-id="8a98d-133">Přihlašovací údaje můžete sdílet mezi aplikací, které jsou podepsány stejný certifikát, poskytování jeden přihlašování pro vaše sada aplikací.</span><span class="sxs-lookup"><span data-stu-id="8a98d-133">Credentials can be shared across applications that are signed by the same certificate, providing a single sign-on experience to your suite of applications.</span></span>
* <span data-ttu-id="8a98d-134">Ovládací prvek kolem možností přihlášení je k dispozici do aplikace před a po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8a98d-134">Control around the experience of logging in is provided to the application before and after sign-in.</span></span>

<span data-ttu-id="8a98d-135">Tyto přihlášení mají tyto nevýhody:</span><span class="sxs-lookup"><span data-stu-id="8a98d-135">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="8a98d-136">Uživatele nelze jednotného přihlašování napříč všechny aplikace, které používají Microsoft Identity pouze v rámci těchto Identities Microsoft, které má vaše aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="8a98d-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="8a98d-137">Aplikaci nelze použít s dalších pokročilých funkcí firmy, jako je například podmíněný přístup, nebo použijte produktů sady InTune.</span><span class="sxs-lookup"><span data-stu-id="8a98d-137">Your application cannot be used with more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="8a98d-138">Aplikace nepodporuje ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="8a98d-139">Zde je reprezentace fungování sadami SDK služby Microsoft Identity se sdíleným úložištěm aplikací k povolení přihlášení SSO:</span><span class="sxs-lookup"><span data-stu-id="8a98d-139">Here is a representation of how the Microsoft Identity SDKs work with the shared storage of your applications to enable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="8a98d-140">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="8a98d-140">Broker assisted logins</span></span>
<span data-ttu-id="8a98d-141">S pomocí zprostředkovatele přihlášení jsou přihlášení prostředí, v rámci zprostředkovatele aplikace, které používají úložiště a security zprostředkovatele sdílet přihlašovací údaje ve všech aplikací v zařízení, které se vztahují na platformu Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="8a98d-141">Broker-assisted logins are login experiences that occur within the broker application and use the storage and security of the broker to share credentials across all applications on the device that apply the Microsoft Identity platform.</span></span> <span data-ttu-id="8a98d-142">To znamená, že vaše aplikace závisí na broker k přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-142">This means that your applications rely on the broker to sign users in.</span></span> <span data-ttu-id="8a98d-143">Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje zařízení pro svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages the device for their user.</span></span> <span data-ttu-id="8a98d-144">Příkladem tento typ aplikace je aplikace Microsoft Authenticator v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="8a98d-144">An example of this type of application is the Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="8a98d-145">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="8a98d-145">In Windows this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>
<span data-ttu-id="8a98d-146">Možnosti se liší podle platformy a v některých případech můžou narušovat běh produktu uživatelům není správně spravovat.</span><span class="sxs-lookup"><span data-stu-id="8a98d-146">The experience varies by platform and can sometimes be disruptive to users if not managed correctly.</span></span> <span data-ttu-id="8a98d-147">Jste pravděpodobně nejvíce obeznámeni s tento vzor, pokud máte nainstalovanou aplikací služby Facebook a použijete Facebook připojit z jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-147">You're probably most familiar with this pattern if you have the Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="8a98d-148">Platforma Microsoft Identity používá stejného vzoru.</span><span class="sxs-lookup"><span data-stu-id="8a98d-148">The Microsoft Identity platform uses the same pattern.</span></span>

<span data-ttu-id="8a98d-149">Pro iOS, které to vede k "přechodu" animace, kdy se vaše aplikace odesílají na pozadí při aplikace Microsoft Authenticator obsahuje popředí pro uživatele k výběru účtu, který se chcete přihlásit.</span><span class="sxs-lookup"><span data-stu-id="8a98d-149">For iOS this leads to a "transition" animation where your application is sent to the background while the Microsoft Authenticator applications comes to the foreground for the user to select which account they would like to sign in with.</span></span>  

<span data-ttu-id="8a98d-150">Pro Android a Windows výběru účtu, zobrazí se na aplikace, což je méně rušivý uživateli.</span><span class="sxs-lookup"><span data-stu-id="8a98d-150">For Android and Windows the account chooser is displayed on top of your application which is less disruptive to the user.</span></span>

#### <a name="how-the-broker-gets-invoked"></a><span data-ttu-id="8a98d-151">Jak se zprostředkovatel volán</span><span class="sxs-lookup"><span data-stu-id="8a98d-151">How the broker gets invoked</span></span>
<span data-ttu-id="8a98d-152">Pokud je kompatibilní zprostředkovatel nainstalovaný na zařízení, jako je aplikace Microsoft Authenticator sadami SDK služby Microsoft Identity automaticky provede práci při vyvolání zprostředkovatele pro vás, když uživatel označuje, že se chcete přihlásit pomocí libovolného účtu z platformy Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="8a98d-152">If a compatible broker is installed on the device, like the Microsoft Authenticator application, the Microsoft Identity SDKs will automatically do the work of invoking the broker for you when a user indicates they wish to log in using any account from the Microsoft Identity platform.</span></span> <span data-ttu-id="8a98d-153">Tento účet může být osobní Account Microsoft, pracovní nebo školní účet, nebo účtu, který zadáte a hostitele v Azure pomocí našich produktů B2C a B2B.</span><span class="sxs-lookup"><span data-stu-id="8a98d-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 
 
 #### <a name="how-we-ensure-the-application-is-valid"></a><span data-ttu-id="8a98d-154">Jakým způsobem je zajištěno aplikace je platný</span><span class="sxs-lookup"><span data-stu-id="8a98d-154">How we ensure the application is valid</span></span>
 
 <span data-ttu-id="8a98d-155">Je třeba zajistit identity volání aplikace, které je zásadní zabezpečení, které poskytujeme ve zprostředkovateli zprostředkovatele s asistencí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8a98d-155">The need to ensure the identity of an application call the broker is crucial to the security we provide in broker assisted logins.</span></span> <span data-ttu-id="8a98d-156">IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny určená pro oprávněné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a98d-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive the tokens meant for the legitimate application.</span></span> <span data-ttu-id="8a98d-157">Ujistěte se, že jsme vždy komunikují pomocí správné aplikace za běhu, požádáme vývojáři poskytnout vlastní redirectURI při registraci své aplikace se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8a98d-157">To ensure we are always communicating with the right application at runtime, we ask the developer to provide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="8a98d-158">**Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.**</span><span class="sxs-lookup"><span data-stu-id="8a98d-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="8a98d-159">Tato vlastní redirectURI obsahuje kryptografický otisk certifikátu aplikace a je zajištěna jedinečnost do aplikace obchod Google Play.</span><span class="sxs-lookup"><span data-stu-id="8a98d-159">This custom redirectURI contains the certificate thumbprint of the application and is ensured to be unique to the application by the Google Play Store.</span></span> <span data-ttu-id="8a98d-160">Pokud aplikace zavolá zprostředkovatele, požádá zprostředkovatele operační systém Android poskytnout kryptografický otisk certifikátu, který volá zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-160">When an application calls the broker, the broker asks the Android operating system to provide it with the certificate thumbprint that called the broker.</span></span> <span data-ttu-id="8a98d-161">Zprostředkovatel poskytuje kryptografický otisk tohoto certifikátu společnosti Microsoft ve volání naše systém identit.</span><span class="sxs-lookup"><span data-stu-id="8a98d-161">The broker provides this certificate thumbprint to Microsoft in the call to our identity system.</span></span> <span data-ttu-id="8a98d-162">Pokud kryptografický otisk certifikátu aplikace neodpovídá kryptografický otisk certifikátu uvedenou nám vývojáře při registraci, jsme odepře přístup na tokeny pro prostředek, který aplikace požaduje.</span><span class="sxs-lookup"><span data-stu-id="8a98d-162">If the certificate thumbprint of the application does not match the certificate thumbprint provided to us by the developer during registration, we will deny access to the tokens for the resource the application is requesting.</span></span> <span data-ttu-id="8a98d-163">Tato kontrola zajistí, že pouze aplikace, které jsou zaregistrované vývojáře přijímá tokeny.</span><span class="sxs-lookup"><span data-stu-id="8a98d-163">This check ensures that only the application registered by the developer receives tokens.</span></span>

<span data-ttu-id="8a98d-164">**Vývojář musí volba, pokud sada SDK Microsoft Identity volá zprostředkovatele nebo používá odbornou toku bez zprostředkovatele.**</span><span class="sxs-lookup"><span data-stu-id="8a98d-164">**The developer has the choice of if the Microsoft Identity SDK calls the broker or uses the non-broker assisted flow.**</span></span> <span data-ttu-id="8a98d-165">Ale pokud vývojář vybere nepoužívat toku s asistencí služby broker nich došlo ke ztrátě výhodou použití jednotného přihlašování k přihlašovací údaje, které uživatel může na zařízení již přidali a zabrání jejich aplikaci z používaný s funkcí business, které společnost Microsoft poskytuje svým zákazníkům, jako je například podmíněný přístup, možnosti správy Intune a ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="8a98d-165">However if the developer chooses not to use the broker-assisted flow they lose the benefit of using SSO credentials that the user may have already added on the device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="8a98d-166">Tyto přihlášení mají následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8a98d-166">These logins have the following benefits:</span></span>

* <span data-ttu-id="8a98d-167">Uživatel vyskytne jednotného přihlašování na všechny svoje aplikace bez ohledu na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-167">User experiences SSO across all their applications no matter the vendor.</span></span>
* <span data-ttu-id="8a98d-168">Aplikace můžete použít dalších pokročilých funkcí firmy, jako je například podmíněný přístup nebo používat sadu InTune produktů.</span><span class="sxs-lookup"><span data-stu-id="8a98d-168">Your application can use more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="8a98d-169">Aplikace může podporovat ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a98d-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="8a98d-170">Mnohem bezpečnější přihlašovat jako identita aplikace a uživatel se ověřit pomocí zprostředkovatele aplikace s další bezpečnostní algoritmů hash a šifrování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-170">Much more secure sign-in experience as the identity of the application and the user are verified by the broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="8a98d-171">Tyto přihlášení mají tyto nevýhody:</span><span class="sxs-lookup"><span data-stu-id="8a98d-171">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="8a98d-172">V iOS převedena uživatele mimo prostředí vaší aplikace, když je možné zvolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8a98d-172">In iOS the user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="8a98d-173">Ztráta schopnost spravovat přihlašování pro vaši zákazníci v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-173">Loss of the ability to manage the login experience for your customers within your application.</span></span>

<span data-ttu-id="8a98d-174">Zde je reprezentace fungování sadami SDK služby Microsoft Identity s aplikacemi zprostředkovatele k povolení přihlášení SSO:</span><span class="sxs-lookup"><span data-stu-id="8a98d-174">Here is a representation of how the Microsoft Identity SDKs work with the broker applications to enable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
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

<span data-ttu-id="8a98d-175">Díky této základní informace, které byste měli mít lépe pochopit a implementovat jednotné přihlašování v rámci vaší aplikace pomocí platformy Microsoft Identity a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="8a98d-175">Armed with this background information you should be able to better understand and implement SSO within your application using the Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="8a98d-176">Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="8a98d-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="8a98d-177">Tady používáme ADAL Android SDK:</span><span class="sxs-lookup"><span data-stu-id="8a98d-177">Here we use the ADAL Android SDK to:</span></span>

* <span data-ttu-id="8a98d-178">Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací</span><span class="sxs-lookup"><span data-stu-id="8a98d-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="8a98d-179">Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="8a98d-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="8a98d-180">Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a98d-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="8a98d-181">Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat sadami SDK služby Microsoft Identity velkou část složitosti jednotného přihlašování pro vás.</span><span class="sxs-lookup"><span data-stu-id="8a98d-181">For non-broker assisted SSO across applications the Microsoft Identity SDKs manage much of the complexity of SSO for you.</span></span> <span data-ttu-id="8a98d-182">To zahrnuje správné uživatelské hledání v mezipaměti a udržování seznam přihlášeného uživatele pro vás k dotazování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-182">This includes finding the right user in the cache and maintaining a list of logged in users for you to query.</span></span>

<span data-ttu-id="8a98d-183">K povolení jednotného přihlašování napříč aplikacemi, které vlastníte, že musíte udělat následující:</span><span class="sxs-lookup"><span data-stu-id="8a98d-183">To enable SSO across applications you own you need to do the following:</span></span>

1. <span data-ttu-id="8a98d-184">Zkontrolujte všechny uživatelské aplikace stejné ID klienta nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-184">Ensure all your applications user the same Client ID or Application ID.</span></span>
2. <span data-ttu-id="8a98d-185">Zajistěte, aby že všechny aplikace mají stejnou sadu SharedUserID.</span><span class="sxs-lookup"><span data-stu-id="8a98d-185">Ensure all your applications have the same SharedUserID set.</span></span>
3. <span data-ttu-id="8a98d-186">Ujistěte se, že všechny aplikace sdílet stejný podpisový certifikát z obchodu Google Play tak, že můžete sdílet úložiště.</span><span class="sxs-lookup"><span data-stu-id="8a98d-186">Ensure that all of your applications share the same signing certificate from the Google Play store so that you can share storage.</span></span>

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a><span data-ttu-id="8a98d-187">Krok 1: Použití stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikací</span><span class="sxs-lookup"><span data-stu-id="8a98d-187">Step 1: Using the same Client ID / Application ID for all the applications in your suite of apps</span></span>
<span data-ttu-id="8a98d-188">V pořadí pro platformu Microsoft Identity vědět, že má povolené sdílet tokeny ve vašich aplikací každý z vašich aplikací bude nutné sdílejí stejné ID klienta nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-188">In order for the Microsoft Identity platform to know that it's allowed to share tokens across your applications, each of your applications will need to share the same Client ID or Application ID.</span></span> <span data-ttu-id="8a98d-189">Toto je jedinečný identifikátor, který jste získali při registraci vaší první aplikace v portálu.</span><span class="sxs-lookup"><span data-stu-id="8a98d-189">This is the unique identifier that was provided to you when you registered your first application in the portal.</span></span>

<span data-ttu-id="8a98d-190">Asi vás zajímá, jak bude identifikujete různé aplikace ke službě Microsoft Identity, pokud používá stejné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="8a98d-190">You may be wondering how you will identify different apps to the Microsoft Identity service if it uses the same Application ID.</span></span> <span data-ttu-id="8a98d-191">Je odpověď **identifikátory URI přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="8a98d-191">The answer is with the **Redirect URIs**.</span></span> <span data-ttu-id="8a98d-192">Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-192">Each application can have multiple Redirect URIs registered in the onboarding portal.</span></span> <span data-ttu-id="8a98d-193">Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-193">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="8a98d-194">Zde je příklad, jak to vypadá:</span><span class="sxs-lookup"><span data-stu-id="8a98d-194">An example of how this looks is below:</span></span>

<span data-ttu-id="8a98d-195">Identifikátor URI přesměrování app1:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span><span class="sxs-lookup"><span data-stu-id="8a98d-195">App1 Redirect URI: `msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span></span>

<span data-ttu-id="8a98d-196">Identifikátor URI přesměrování počítači App2:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span><span class="sxs-lookup"><span data-stu-id="8a98d-196">App2 Redirect URI: `msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span></span>

<span data-ttu-id="8a98d-197">Identifikátor URI přesměrování App3:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span><span class="sxs-lookup"><span data-stu-id="8a98d-197">App3 Redirect URI: `msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span></span>

<span data-ttu-id="8a98d-198">....</span><span class="sxs-lookup"><span data-stu-id="8a98d-198">....</span></span>

<span data-ttu-id="8a98d-199">Tyto jsou vnořeny pod stejným ID klienta nebo ID aplikace a vyhledávat podle identifikátor URI návratu do us ve vaší konfiguraci SDK přesměrování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-199">These are nested under the same client ID / application ID and looked up based on the redirect URI you return to us in your SDK configuration.</span></span>

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


<span data-ttu-id="8a98d-200">*Všimněte si, že formát tyto identifikátory URI přesměrování jsou vysvětleny níže. Můžete použít všechny URI přesměrování, pokud chcete pro podporu zprostředkovatele, v takovém případě se musí vypadat podobně jako výše*</span><span class="sxs-lookup"><span data-stu-id="8a98d-200">*Note that the format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish to support the broker, in which case they must look something like the above*</span></span>

#### <a name="step-2-configuring-shared-storage-in-android"></a><span data-ttu-id="8a98d-201">Krok 2: Konfigurace sdílené úložiště v Android</span><span class="sxs-lookup"><span data-stu-id="8a98d-201">Step 2: Configuring shared storage in Android</span></span>
<span data-ttu-id="8a98d-202">Nastavení `SharedUserID` je nad rámec tohoto dokumentu, ale je možné zjistit přečíst v dokumentaci Google Android na [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span><span class="sxs-lookup"><span data-stu-id="8a98d-202">Setting the `SharedUserID` is beyond the scope of this document but can be learned by reading the Google Android documentation on the [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span></span> <span data-ttu-id="8a98d-203">Co je důležité se rozhodnout, jaké má vaše sharedUserID bude volána a použít na všechny aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-203">What is important is that you decide what you want your sharedUserID will be called and use that across all your applications.</span></span>

<span data-ttu-id="8a98d-204">Jakmile máte `SharedUserID` ve svých aplikacích budete chtít používat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-204">Once you have the `SharedUserID` in all your applications you are ready to use SSO.</span></span>

> [!WARNING]
> <span data-ttu-id="8a98d-205">Při sdílení úložišť na vaší aplikace pro všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny napříč aplikací.</span><span class="sxs-lookup"><span data-stu-id="8a98d-205">When you share storage across your applications any application can delete users, or worse delete all the tokens across your application.</span></span> <span data-ttu-id="8a98d-206">To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na tokeny pro práci na pozadí.</span><span class="sxs-lookup"><span data-stu-id="8a98d-206">This is particularly disastrous if you have applications that rely on the tokens to do background work.</span></span> <span data-ttu-id="8a98d-207">Sdílení úložiště znamená, že musí být velmi opatrní při odebrání všech operací prostřednictvím sadami SDK služby Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="8a98d-207">Sharing storage means that you must be very careful in any and all remove operations through the Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="8a98d-208">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="8a98d-208">That's it!</span></span> <span data-ttu-id="8a98d-209">Sada SDK Microsoft Identity bude nyní sdílet přihlašovací údaje ve všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="8a98d-209">The Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="8a98d-210">Seznam uživatelů bude také sdílet mezi instancemi aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-210">The user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="8a98d-211">Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a98d-211">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="8a98d-212">Možnost pro aplikace pro použití žádné zprostředkovatele, který je nainstalován na zařízení je **ve výchozím nastavení vypnuté**.</span><span class="sxs-lookup"><span data-stu-id="8a98d-212">The ability for an application to use any broker that is installed on the device is **turned off by default**.</span></span> <span data-ttu-id="8a98d-213">Chcete-li použít vaší aplikace pomocí zprostředkovatele musí provést některé další konfiguraci a přidejte nějaký kód do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-213">In order to use your application with the broker you must do some additional configuration and add some code to your application.</span></span>

<span data-ttu-id="8a98d-214">Jak postupovat, jsou:</span><span class="sxs-lookup"><span data-stu-id="8a98d-214">The steps to follow are:</span></span>

1. <span data-ttu-id="8a98d-215">Povolit režim zprostředkovatele v kódu aplikace volání sady SDK MS</span><span class="sxs-lookup"><span data-stu-id="8a98d-215">Enable broker mode in your application code's call to the MS SDK</span></span>
2. <span data-ttu-id="8a98d-216">Vytvořit nový identifikátor URI přesměrování a stanovit, že aplikace a registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="8a98d-216">Establish a new redirect URI and provide that to both the app and your app registration</span></span>
3. <span data-ttu-id="8a98d-217">Nastavení správná oprávnění v manifestu systému Android.</span><span class="sxs-lookup"><span data-stu-id="8a98d-217">Setting up the correct permissions in the Android manifest</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="8a98d-218">Krok 1: Povolení režimu zprostředkovatele v aplikaci</span><span class="sxs-lookup"><span data-stu-id="8a98d-218">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="8a98d-219">Možnosti pro aplikace pomocí zprostředkovatele zapnutý, při vytváření "nastavení" nebo počáteční nastavení vaší instance ověřování.</span><span class="sxs-lookup"><span data-stu-id="8a98d-219">The ability for your application to use the broker is turned on when you create the "settings" or initial setup of your Authentication instance.</span></span> <span data-ttu-id="8a98d-220">To provedete nastavením vašeho typu ApplicationSettings ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="8a98d-220">You do this by setting your ApplicationSettings type in your code:</span></span>

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="8a98d-221">Krok 2: Vytvoření na nový identifikátor URI s vaše schéma adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="8a98d-221">Step 2: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="8a98d-222">Aby se zajistilo, že vrátíme vždy tokeny přihlašovacích údajů pro správnou aplikaci, musíme Ujistěte se, že jsme zpětné volání do vaší aplikace tak, aby operační systém Android můžete ověřit.</span><span class="sxs-lookup"><span data-stu-id="8a98d-222">In order to ensure that we always return the credential tokens to the correct application, we need to make sure we call back to your application in a way that the Android operating system can verify.</span></span> <span data-ttu-id="8a98d-223">Operační systém Android používá hodnotu hash certifikátu v obchodě Google Play.</span><span class="sxs-lookup"><span data-stu-id="8a98d-223">The Android operating system uses the hash of the certificate in the Google Play store.</span></span> <span data-ttu-id="8a98d-224">To nelze maskování podvodný aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-224">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="8a98d-225">Proto jsme využít to společně se identifikátor URI aplikace zprostředkovatele k zajištění, že tokeny jsou vráceny správné aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a98d-225">Therefore, we leverage this along with the URI of our broker application to ensure that the tokens are returned to the correct application.</span></span> <span data-ttu-id="8a98d-226">Je nutné vytvořit tento jedinečný identifikátor URI pro přesměrování oba ve vaší aplikaci a nastavit jako identifikátor URI přesměrování v našem portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="8a98d-226">We require you to establish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="8a98d-227">Váš identifikátor URI pro přesměrování musí být ve správném formátu:</span><span class="sxs-lookup"><span data-stu-id="8a98d-227">Your redirect URI must be in the proper form of:</span></span>

`msauth://packagename/Base64UrlencodedSignature`

<span data-ttu-id="8a98d-228">například: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span><span class="sxs-lookup"><span data-stu-id="8a98d-228">ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span></span>

<span data-ttu-id="8a98d-229">Tento identifikátor URI pro přesměrování musí být zadána v registrace vaší aplikace pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8a98d-229">This Redirect URI needs to be specified in your app registration using the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="8a98d-230">Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="8a98d-230">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a><span data-ttu-id="8a98d-231">Krok 3: Nastavení správná oprávnění v aplikaci</span><span class="sxs-lookup"><span data-stu-id="8a98d-231">Step 3: Set up the correct permissions in your application</span></span>
<span data-ttu-id="8a98d-232">Naše aplikace zprostředkovatele v Android používá funkci Správce účtů operační systém Android ke správě přihlašovacích údajů napříč aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="8a98d-232">Our broker application in Android uses the Accounts Manager feature of the Android OS to manage credentials across applications.</span></span> <span data-ttu-id="8a98d-233">Chcete-li použít zprostředkovatele v Android manifest aplikace musí mít oprávnění k používání AccountManager účtů.</span><span class="sxs-lookup"><span data-stu-id="8a98d-233">In order to use the broker in Android your app manifest must have permissions to use AccountManager accounts.</span></span> <span data-ttu-id="8a98d-234">To je podrobněji v [zde Google dokumentace pro účet správce](http://developer.android.com/reference/android/accounts/AccountManager.html)</span><span class="sxs-lookup"><span data-stu-id="8a98d-234">This is discussed in detail in the [Google documentation for Account Manager here](http://developer.android.com/reference/android/accounts/AccountManager.html)</span></span>

<span data-ttu-id="8a98d-235">Konkrétně tato oprávnění jsou:</span><span class="sxs-lookup"><span data-stu-id="8a98d-235">In particular, these permissions are:</span></span>

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a><span data-ttu-id="8a98d-236">Jednotné přihlašování jste nakonfigurovali!</span><span class="sxs-lookup"><span data-stu-id="8a98d-236">You've configured SSO!</span></span>
<span data-ttu-id="8a98d-237">Nyní Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele, pokud je k dispozici na svém zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a98d-237">Now the Microsoft Identity SDK will automatically both share credentials across your applications and invoke the broker if it's present on their device.</span></span>

