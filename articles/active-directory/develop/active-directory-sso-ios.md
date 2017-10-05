---
title: "Postup povolení jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL | Microsoft Docs"
description: "Jak používat funkce sady ADAL SDK povolit jednotné přihlašování v rámci vaší aplikace. "
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
ms.openlocfilehash: 73b8ed7e6a153a0790f7eae9bd51bb2e554ae72e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a><span data-ttu-id="ae6b0-103">Postup povolení jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="ae6b0-103">How to enable cross-app SSO on iOS using ADAL</span></span>
<span data-ttu-id="ae6b0-104">Pokud jednotné přihlašování (SSO), aby uživatelé stačí jednou zadat své přihlašovací údaje a mají tyto přihlašovací údaje automaticky fungovat na všech aplikací nyní očekává zákazníků.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-104">Providing Single Sign-On (SSO) so that users only need to enter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="ae6b0-105">Problémy se zadáním uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má k tomu více než jednou pro svůj produkt.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-105">The difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has to do this more than one time for your product.</span></span>

<span data-ttu-id="ae6b0-106">Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávají, že ty pověření být k dispozici pro použití na všechny svoje aplikace bez ohledu na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials to be available to use across all their applications no matter the vendor.</span></span>

<span data-ttu-id="ae6b0-107">Do platformy Microsoft Identity, společně s naše sady SDK Identity Microsoft nepodporuje tento pevný pracovní pro vás a vám dává možnost delight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sada aplikací nebo, stejně jako u našich schopností zprostředkovatele a ověřovací aplikacemi, v rámci celého zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-107">The Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you the ability to delight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across the entire device.</span></span>

<span data-ttu-id="ae6b0-108">Tento návod popisuje postup konfigurace naše sady SDK v rámci aplikace umožní vašim zákazníkům poskytovat této výhody.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-108">This walkthrough will tell you how to configure our SDK within your application to provide this benefit to your customers.</span></span>

<span data-ttu-id="ae6b0-109">Tento postup platí pro:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="ae6b0-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae6b0-110">Azure Active Directory</span></span>
* <span data-ttu-id="ae6b0-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="ae6b0-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="ae6b0-112">Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="ae6b0-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="ae6b0-113">Podmíněný přístup k Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae6b0-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="ae6b0-114">Předchozí dokument předpokládá, že víte, jak [zřídit aplikace na portálu pro starší verze pro Azure Active Directory](active-directory-how-to-integrate.md) a integrované aplikace s [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span><span class="sxs-lookup"><span data-stu-id="ae6b0-114">The document preceding assumes you know how to [provision applications in the legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with the [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span></span>

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a><span data-ttu-id="ae6b0-115">Koncepty jednotné přihlašování v platformě Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="ae6b0-115">SSO Concepts in the Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="ae6b0-116">Zprostředkovatelé Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="ae6b0-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="ae6b0-117">Společnost Microsoft poskytuje aplikace pro každou platformu mobilních umožňujících přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud se ověřit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-117">Microsoft provides applications for every mobile platform that allow for the bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where to validate credentials.</span></span> <span data-ttu-id="ae6b0-118">Tyto říkáme **makléřům**.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-118">We call these **brokers**.</span></span> <span data-ttu-id="ae6b0-119">Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje některých nebo všech zařízení pro své zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-119">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages some or all of the device for their employee.</span></span> <span data-ttu-id="ae6b0-120">Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo ze zařízení podle potřeby co správci IT.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-120">These brokers support managing security just for some applications or the entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="ae6b0-121">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-121">In Windows, this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>

<span data-ttu-id="ae6b0-122">Další informace o tom, použijeme tyto zprostředkovatelé a jak vaši zákazníci mohou zobrazit je v jejich toku přihlášení pro čtení na platformu Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-122">For more information on how we use these brokers and how your customers might see them in their login flow for the Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="ae6b0-123">Vzory pro přihlašování na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="ae6b0-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="ae6b0-124">Přístup k přihlašovacím údajům v zařízení podle dva základní vzory pro platformu Microsoft Identity:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-124">Access to credentials on devices follow two basic patterns for the Microsoft Identity platform:</span></span>

* <span data-ttu-id="ae6b0-125">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="ae6b0-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="ae6b0-126">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="ae6b0-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="ae6b0-127">Přihlášení odbornou bez zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="ae6b0-127">Non-broker assisted logins</span></span>
<span data-ttu-id="ae6b0-128">Přihlášení odbornou non-broker jsou možností přihlášení, které dojít vložené s aplikací a použít místní úložiště na zařízení pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-128">Non-broker assisted logins are login experiences that happen inline with the application and use the local storage on the device for that application.</span></span> <span data-ttu-id="ae6b0-129">Toto úložiště může být sdíleny s aplikací, ale přihlašovací údaje jsou úzce vázaný aplikace nebo sadu aplikací pomocí tohoto pověření.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-129">This storage may be shared across applications but the credentials are tightly bound to the app or suite of apps using that credential.</span></span> <span data-ttu-id="ae6b0-130">Jste s největší pravděpodobností došlo k to v mnoha mobilních aplikacích. když zadáte uživatelské jméno a heslo v rámci vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-130">You've most likely experienced this in many mobile applications when you enter a username and password within the application itself.</span></span>

<span data-ttu-id="ae6b0-131">Tyto přihlášení mají následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-131">These logins have the following benefits:</span></span>

* <span data-ttu-id="ae6b0-132">Činnost koncového uživatele zcela v aplikaci existuje.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-132">User experience exists entirely within the application.</span></span>
* <span data-ttu-id="ae6b0-133">Přihlašovací údaje můžete sdílet mezi aplikací, které jsou podepsány stejný certifikát, poskytování jeden přihlašování pro vaše sada aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-133">Credentials can be shared across applications that are signed by the same certificate, providing a single sign-on experience to your suite of applications.</span></span>
* <span data-ttu-id="ae6b0-134">Ovládací prvek kolem možností přihlášení je k dispozici do aplikace před a po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-134">Control around the experience of logging in is provided to the application before and after sign-in.</span></span>

<span data-ttu-id="ae6b0-135">Tyto přihlášení mají tyto nevýhody:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-135">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="ae6b0-136">Uživatele nelze jednotného přihlašování napříč všechny aplikace, které používají Microsoft Identity pouze v rámci těchto Identities Microsoft, které má vaše aplikace nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="ae6b0-137">Aplikaci nelze použít s dalších pokročilých funkcí firmy, jako je například podmíněný přístup, nebo použijte produktů sady InTune.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-137">Your application cannot be used with more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="ae6b0-138">Aplikace nepodporuje ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="ae6b0-139">Zde je reprezentace fungování sadami SDK služby Microsoft Identity se sdíleným úložištěm aplikací k povolení přihlášení SSO:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-139">Here is a representation of how the Microsoft Identity SDKs work with the shared storage of your applications to enable SSO:</span></span>

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

#### <a name="broker-assisted-logins"></a><span data-ttu-id="ae6b0-140">Zprostředkovatel odbornou přihlášení</span><span class="sxs-lookup"><span data-stu-id="ae6b0-140">Broker assisted logins</span></span>
<span data-ttu-id="ae6b0-141">S pomocí zprostředkovatele přihlášení jsou přihlášení prostředí, v rámci zprostředkovatele aplikace, které používají úložiště a security zprostředkovatele sdílet přihlašovací údaje ve všech aplikací v zařízení, které se vztahují na platformu Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-141">Broker-assisted logins are login experiences that occur within the broker application and use the storage and security of the broker to share credentials across all applications on the device that apply the Microsoft Identity platform.</span></span> <span data-ttu-id="ae6b0-142">To znamená, že vaše aplikace závisí na broker k přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-142">This means that your applications rely on the broker to sign users in.</span></span> <span data-ttu-id="ae6b0-143">Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje zařízení pro svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages the device for their user.</span></span> <span data-ttu-id="ae6b0-144">Příkladem tento typ aplikace je aplikace Microsoft Authenticator v systému iOS.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-144">An example of this type of application is the Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="ae6b0-145">V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-145">In Windows this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>
<span data-ttu-id="ae6b0-146">Možnosti se liší podle platformy a v některých případech můžou narušovat běh produktu uživatelům není správně spravovat.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-146">The experience varies by platform and can sometimes be disruptive to users if not managed correctly.</span></span> <span data-ttu-id="ae6b0-147">Jste pravděpodobně nejvíce obeznámeni s tento vzor, pokud máte nainstalovanou aplikací služby Facebook a použijete Facebook připojit z jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-147">You're probably most familiar with this pattern if you have the Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="ae6b0-148">Platforma Microsoft Identity používá stejného vzoru.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-148">The Microsoft Identity platform uses the same pattern.</span></span>

<span data-ttu-id="ae6b0-149">Pro iOS, které to vede k "přechodu" animace, kdy se vaše aplikace odesílají na pozadí při aplikace Microsoft Authenticator obsahuje popředí pro uživatele k výběru účtu, který se chcete přihlásit.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-149">For iOS this leads to a "transition" animation where your application is sent to the background while the Microsoft Authenticator applications comes to the foreground for the user to select which account they would like to sign in with.</span></span>  

<span data-ttu-id="ae6b0-150">Pro Android a Windows výběru účtu, zobrazí se na aplikace, což je méně rušivý uživateli.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-150">For Android and Windows the account chooser is displayed on top of your application which is less disruptive to the user.</span></span>

#### <a name="how-the-broker-gets-invoked"></a><span data-ttu-id="ae6b0-151">Jak se zprostředkovatel volán</span><span class="sxs-lookup"><span data-stu-id="ae6b0-151">How the broker gets invoked</span></span>
<span data-ttu-id="ae6b0-152">Pokud je kompatibilní zprostředkovatel nainstalovaný na zařízení, jako je aplikace Microsoft Authenticator sadami SDK služby Microsoft Identity automaticky provede práci při vyvolání zprostředkovatele pro vás, když uživatel označuje, že se chcete přihlásit pomocí libovolného účtu z platformy Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-152">If a compatible broker is installed on the device, like the Microsoft Authenticator application, the Microsoft Identity SDKs will automatically do the work of invoking the broker for you when a user indicates they wish to log in using any account from the Microsoft Identity platform.</span></span> <span data-ttu-id="ae6b0-153">Tento účet může být osobní Account Microsoft, pracovní nebo školní účet, nebo účtu, který zadáte a hostitele v Azure pomocí našich produktů B2C a B2B.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 

 #### <a name="how-we-ensure-the-application-is-valid"></a><span data-ttu-id="ae6b0-154">Jakým způsobem je zajištěno aplikace je platný</span><span class="sxs-lookup"><span data-stu-id="ae6b0-154">How we ensure the application is valid</span></span>
 
 <span data-ttu-id="ae6b0-155">Je třeba zajistit identity volání aplikace, které je zásadní zabezpečení, které poskytujeme ve zprostředkovateli zprostředkovatele s asistencí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-155">The need to ensure the identity of an application call the broker is crucial to the security we provide in broker assisted logins.</span></span> <span data-ttu-id="ae6b0-156">IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny určená pro oprávněné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive the tokens meant for the legitimate application.</span></span> <span data-ttu-id="ae6b0-157">Ujistěte se, že jsme vždy komunikují pomocí správné aplikace za běhu, požádáme vývojáři poskytnout vlastní redirectURI při registraci své aplikace se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-157">To ensure we are always communicating with the right application at runtime, we ask the developer to provide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="ae6b0-158">**Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.**</span><span class="sxs-lookup"><span data-stu-id="ae6b0-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="ae6b0-159">Tato vlastní redirectURI obsahuje ID sady prostředků aplikace a je zajištěna jedinečnost k aplikaci Apple App Storu.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-159">This custom redirectURI contains the Bundle ID of the application and is ensured to be unique to the application by the Apple App Store.</span></span> <span data-ttu-id="ae6b0-160">Pokud aplikace zavolá zprostředkovatele, požádá zprostředkovatele operačního systému iOS poskytnout ID sady, který volá zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-160">When an application calls the broker, the broker asks the iOS operating system to provide it with the Bundle ID that called the broker.</span></span> <span data-ttu-id="ae6b0-161">Zprostředkovatel poskytuje toto ID sady prostředků společnosti Microsoft ve volání naše systém identit.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-161">The broker provides this Bundle ID to Microsoft in the call to our identity system.</span></span> <span data-ttu-id="ae6b0-162">Pokud ID sady prostředků aplikace neodpovídá ID sady, které vývojář poskytuje nám během registrace, jsme odepře přístup k tokeny pro prostředek aplikace požaduje.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-162">If the Bundle ID of the application does not match the Bundle ID provided to us by the developer during registration, we will deny access to the tokens for the resource the application is requesting.</span></span> <span data-ttu-id="ae6b0-163">Tato kontrola zajistí, že pouze aplikace, které jsou zaregistrované vývojáře přijímá tokeny.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-163">This check ensures that only the application registered by the developer receives tokens.</span></span>

<span data-ttu-id="ae6b0-164">**Vývojář musí volba, pokud sada SDK Microsoft Identity volá zprostředkovatele nebo používá odbornou toku bez zprostředkovatele.**</span><span class="sxs-lookup"><span data-stu-id="ae6b0-164">**The developer has the choice of if the Microsoft Identity SDK calls the broker or uses the non-broker assisted flow.**</span></span> <span data-ttu-id="ae6b0-165">Ale pokud vývojář vybere nepoužívat toku s asistencí služby broker nich došlo ke ztrátě výhodou použití jednotného přihlašování k přihlašovací údaje, které uživatel může na zařízení již přidali a zabrání jejich aplikaci z používaný s funkcí business, které společnost Microsoft poskytuje svým zákazníkům, jako je například podmíněný přístup, možnosti správy Intune a ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-165">However if the developer chooses not to use the broker-assisted flow they lose the benefit of using SSO credentials that the user may have already added on the device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="ae6b0-166">Tyto přihlášení mají následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-166">These logins have the following benefits:</span></span>

* <span data-ttu-id="ae6b0-167">Uživatel vyskytne jednotného přihlašování na všechny svoje aplikace bez ohledu na dodavatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-167">User experiences SSO across all their applications no matter the vendor.</span></span>
* <span data-ttu-id="ae6b0-168">Aplikace můžete použít dalších pokročilých funkcí firmy, jako je například podmíněný přístup nebo používat sadu InTune produktů.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-168">Your application can use more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="ae6b0-169">Aplikace může podporovat ověřování pomocí certifikátů pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="ae6b0-170">Mnohem bezpečnější přihlašovat jako identita aplikace a uživatel se ověřit pomocí zprostředkovatele aplikace s další bezpečnostní algoritmů hash a šifrování.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-170">Much more secure sign-in experience as the identity of the application and the user are verified by the broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="ae6b0-171">Tyto přihlášení mají tyto nevýhody:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-171">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="ae6b0-172">V iOS převedena uživatele mimo prostředí vaší aplikace, když je možné zvolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-172">In iOS the user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="ae6b0-173">Ztráta schopnost spravovat přihlašování pro vaši zákazníci v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-173">Loss of the ability to manage the login experience for your customers within your application.</span></span>

<span data-ttu-id="ae6b0-174">Zde je reprezentace fungování sadami SDK služby Microsoft Identity s aplikacemi zprostředkovatele k povolení přihlášení SSO:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-174">Here is a representation of how the Microsoft Identity SDKs work with the broker applications to enable SSO:</span></span>

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

<span data-ttu-id="ae6b0-175">Díky této základní informace, které byste měli mít lépe pochopit a implementovat jednotné přihlašování v rámci vaší aplikace pomocí platformy Microsoft Identity a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-175">Armed with this background information you should be able to better understand and implement SSO within your application using the Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="ae6b0-176">Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="ae6b0-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="ae6b0-177">Tady používáme ADAL iOS SDK na:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-177">Here we use the ADAL iOS SDK to:</span></span>

* <span data-ttu-id="ae6b0-178">Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací</span><span class="sxs-lookup"><span data-stu-id="ae6b0-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="ae6b0-179">Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="ae6b0-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="ae6b0-180">Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae6b0-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="ae6b0-181">Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat sadami SDK služby Microsoft Identity velkou část složitosti jednotného přihlašování pro vás.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-181">For non-broker assisted SSO across applications the Microsoft Identity SDKs manage much of the complexity of SSO for you.</span></span> <span data-ttu-id="ae6b0-182">To zahrnuje správné uživatelské hledání v mezipaměti a udržování seznam přihlášeného uživatele pro vás k dotazování.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-182">This includes finding the right user in the cache and maintaining a list of logged in users for you to query.</span></span>

<span data-ttu-id="ae6b0-183">K povolení jednotného přihlašování napříč aplikacemi, které vlastníte, že musíte udělat následující:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-183">To enable SSO across applications you own you need to do the following:</span></span>

1. <span data-ttu-id="ae6b0-184">Zkontrolujte všechny uživatelské aplikace stejné ID klienta nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-184">Ensure all your applications user the same Client ID or Application ID.</span></span>
2. <span data-ttu-id="ae6b0-185">Ujistěte se, že všechny aplikace sdílet stejný podpisový certifikát od společnosti Apple tak, že můžete sdílet řetězce klíčů</span><span class="sxs-lookup"><span data-stu-id="ae6b0-185">Ensure that all of your applications share the same signing certificate from Apple so that you can share keychains</span></span>
3. <span data-ttu-id="ae6b0-186">Požádat o stejné nárocích řetězce klíčů pro každou z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-186">Request the same keychain entitlement for each of your applications.</span></span>
4. <span data-ttu-id="ae6b0-187">Chcete, abychom mohli používat, informujte o sdíleném řetězci klíčů sadami SDK služby Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-187">Tell the Microsoft Identity SDKs about the shared keychain you want us to use.</span></span>

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a><span data-ttu-id="ae6b0-188">Pomocí stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikací</span><span class="sxs-lookup"><span data-stu-id="ae6b0-188">Using the same Client ID / Application ID for all the applications in your suite of apps</span></span>
<span data-ttu-id="ae6b0-189">V pořadí pro platformu Microsoft Identity vědět, že má povolené sdílet tokeny ve vašich aplikací každý z vašich aplikací bude nutné sdílejí stejné ID klienta nebo ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-189">In order for the Microsoft Identity platform to know that it's allowed to share tokens across your applications, each of your applications will need to share the same Client ID or Application ID.</span></span> <span data-ttu-id="ae6b0-190">Toto je jedinečný identifikátor, který jste získali při registraci vaší první aplikace v portálu.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-190">This is the unique identifier that was provided to you when you registered your first application in the portal.</span></span>

<span data-ttu-id="ae6b0-191">Asi vás zajímá, jak bude identifikujete různé aplikace ke službě Microsoft Identity, pokud používá stejné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="ae6b0-191">You may be wondering how you will identify different apps to the Microsoft Identity service if it uses the same Application ID.</span></span> <span data-ttu-id="ae6b0-192">Je odpověď **identifikátory URI přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-192">The answer is with the **Redirect URIs**.</span></span> <span data-ttu-id="ae6b0-193">Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-193">Each application can have multiple Redirect URIs registered in the onboarding portal.</span></span> <span data-ttu-id="ae6b0-194">Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-194">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="ae6b0-195">Zde je příklad, jak to vypadá:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-195">An example of how this looks is below:</span></span>

<span data-ttu-id="ae6b0-196">Identifikátor URI přesměrování app1:`x-msauth-mytestiosapp://com.myapp.mytestapp`</span><span class="sxs-lookup"><span data-stu-id="ae6b0-196">App1 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`</span></span>

<span data-ttu-id="ae6b0-197">Identifikátor URI přesměrování počítači App2:`x-msauth-mytestiosapp://com.myapp.mytestapp2`</span><span class="sxs-lookup"><span data-stu-id="ae6b0-197">App2 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`</span></span>

<span data-ttu-id="ae6b0-198">Identifikátor URI přesměrování App3:`x-msauth-mytestiosapp://com.myapp.mytestapp3`</span><span class="sxs-lookup"><span data-stu-id="ae6b0-198">App3 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`</span></span>

<span data-ttu-id="ae6b0-199">....</span><span class="sxs-lookup"><span data-stu-id="ae6b0-199">....</span></span>

<span data-ttu-id="ae6b0-200">Tyto jsou vnořeny pod stejným ID klienta nebo ID aplikace a vyhledávat podle identifikátor URI návratu do us ve vaší konfiguraci SDK přesměrování.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-200">These are nested under the same client ID / application ID and looked up based on the redirect URI you return to us in your SDK configuration.</span></span>

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


<span data-ttu-id="ae6b0-201">*Všimněte si, že formát tyto identifikátory URI přesměrování jsou vysvětleny níže. Můžete použít všechny URI přesměrování, pokud chcete pro podporu zprostředkovatele, v takovém případě se musí vypadat podobně jako výše*</span><span class="sxs-lookup"><span data-stu-id="ae6b0-201">*Note that the format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish to support the broker, in which case they must look something like the above*</span></span>

#### <a name="create-keychain-sharing-between-applications"></a><span data-ttu-id="ae6b0-202">Vytvořte sdílení mezi aplikacemi řetězce klíčů</span><span class="sxs-lookup"><span data-stu-id="ae6b0-202">Create keychain sharing between applications</span></span>
<span data-ttu-id="ae6b0-203">Povolení sdílení řetězce klíčů je nad rámec tohoto dokumentu a jejich dokument nezabývá společností Apple [přidání schopností](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span><span class="sxs-lookup"><span data-stu-id="ae6b0-203">Enabling keychain sharing is beyond the scope of this document and covered by Apple in their document [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span></span> <span data-ttu-id="ae6b0-204">Co je důležité se rozhodnout, co chcete klíčů a k volání a přidejte tuto funkci u všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-204">What is important is that you decide what you want your keychain to be called and add that capability across all your applications.</span></span>

<span data-ttu-id="ae6b0-205">Pokud máte oprávnění nastavit správně jste měli vidět soubor v adresáři projektu s názvem `entitlements.plist` obsahující objekt, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-205">When you do have entitlements set up correctly you should see a file in your project directory entitled `entitlements.plist` that contains something that looks like the following:</span></span>

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

<span data-ttu-id="ae6b0-206">Jakmile máte nárok řetězce klíčů povolené v každé z vašich aplikací a jste připraveni používat jednotné přihlašování, říct sadě SDK, Identity Microsoft o vaší řetězce klíčů pomocí následující nastavení v vaší `ADAuthenticationSettings` s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-206">Once you have the keychain entitlement enabled in each of your applications, and you are ready to use SSO, tell the Microsoft Identity SDK about your keychain by using the following setting in your `ADAuthenticationSettings` with the following setting:</span></span>

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> <span data-ttu-id="ae6b0-207">Když sdílíte mezi vaší aplikace řetězce klíčů všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny napříč aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-207">When you share a keychain across your applications any application can delete users or worse delete all the tokens across your application.</span></span> <span data-ttu-id="ae6b0-208">To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na tokeny pro práci na pozadí.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-208">This is particularly disastrous if you have applications that rely on the tokens to do background work.</span></span> <span data-ttu-id="ae6b0-209">Sdílení řetězce klíčů odebrat znamená, že musí být velmi opatrní při všech operací prostřednictvím sady SDK Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-209">Sharing a keychain means that you must be very careful in any and all remove operations through the Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="ae6b0-210">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="ae6b0-210">That's it!</span></span> <span data-ttu-id="ae6b0-211">Sada SDK Microsoft Identity bude nyní sdílet přihlašovací údaje ve všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-211">The Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="ae6b0-212">Seznam uživatelů bude také sdílet mezi instancemi aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-212">The user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="ae6b0-213">Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae6b0-213">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="ae6b0-214">Možnost pro aplikace pro použití žádné zprostředkovatele, který je nainstalován na zařízení je **ve výchozím nastavení vypnuté**.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-214">The ability for an application to use any broker that is installed on the device is **turned off by default**.</span></span> <span data-ttu-id="ae6b0-215">Chcete-li použít vaší aplikace pomocí zprostředkovatele musí provést některé další konfiguraci a přidejte nějaký kód do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-215">In order to use your application with the broker you must do some additional configuration and add some code to your application.</span></span>

<span data-ttu-id="ae6b0-216">Jak postupovat, jsou:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-216">The steps to follow are:</span></span>

1. <span data-ttu-id="ae6b0-217">Povolte režim zprostředkovatele v kódu aplikace volání sady SDK MS.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-217">Enable broker mode in your application code's call to the MS SDK.</span></span>
2. <span data-ttu-id="ae6b0-218">Vytvořit nový identifikátor URI přesměrování a stanovit, že aplikace a registrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-218">Establish a new redirect URI and provide that to both the app and your app registration.</span></span>
3. <span data-ttu-id="ae6b0-219">Probíhá registrace schéma adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-219">Registering a URL Scheme.</span></span>
4. <span data-ttu-id="ae6b0-220">Podpora iOS9: přidejte oprávnění do souboru info.plist.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-220">iOS9 Support: Add a permission to your info.plist file.</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="ae6b0-221">Krok 1: Povolení režimu zprostředkovatele v aplikaci</span><span class="sxs-lookup"><span data-stu-id="ae6b0-221">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="ae6b0-222">Možnosti pro aplikace pomocí zprostředkovatele zapnutý, při vytváření "context" nebo počáteční nastavení ověřování objektu.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-222">The ability for your application to use the broker is turned on when you create the "context" or initial setup of your Authentication object.</span></span> <span data-ttu-id="ae6b0-223">To provedete nastavením typ vaše přihlašovací údaje ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-223">You do this by setting your credentials type in your code:</span></span>

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
<span data-ttu-id="ae6b0-224">`AD_CREDENTIALS_AUTO` Nastavení umožní Microsoft Identity SDK můžete vyzkoušet vyvolávající pro zprostředkovatele, `AD_CREDENTIALS_EMBEDDED` Microsoft Identity SDK zabrání volání pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-224">The `AD_CREDENTIALS_AUTO` setting will allow the Microsoft Identity SDK to try to call out to the broker, `AD_CREDENTIALS_EMBEDDED` will prevent the Microsoft Identity SDK from calling to the broker.</span></span>

#### <a name="step-2-registering-a-url-scheme"></a><span data-ttu-id="ae6b0-225">Krok 2: Registrace schéma adresy URL</span><span class="sxs-lookup"><span data-stu-id="ae6b0-225">Step 2: Registering a URL Scheme</span></span>
<span data-ttu-id="ae6b0-226">Platforma Microsoft Identity adresy URL používá k vyvolání zprostředkovatele a pak se vraťte řízení zpět do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-226">The Microsoft Identity platform uses URLs to invoke the broker and then return control back to your application.</span></span> <span data-ttu-id="ae6b0-227">K dokončení této odezvy musíte zaregistrovat pro vaše aplikace, která bude platformou Microsoft Identity vědět o schéma adres URL.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-227">To finish that round trip you need a URL scheme registered for your application that the Microsoft Identity platform will know about.</span></span> <span data-ttu-id="ae6b0-228">To může být kromě jiných aplikace režimů, které mají může dříve registrován u vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-228">This can be in addition to any other app schemes you may have previously registered with your application.</span></span>

> [!WARNING]
> <span data-ttu-id="ae6b0-229">Doporučujeme schématu adresy URL poměrně jedinečný, chcete-li minimalizovat pravděpodobnost jiné aplikaci pomocí stejné schéma adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-229">We recommend making the URL scheme fairly unique to minimize the chances of another app using the same URL scheme.</span></span> <span data-ttu-id="ae6b0-230">Apple nevynucuje jedinečnost schémata URL, které jsou zaregistrovány v app storu.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-230">Apple does not enforce the uniqueness of URL schemes that are registered in the app store.</span></span>
> 
> 

<span data-ttu-id="ae6b0-231">Níže je příklad, jak se objeví v konfigurace projektu.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-231">Below is an example of how this appears in your project configuration.</span></span> <span data-ttu-id="ae6b0-232">Také to lze provést v prostředí XCode také:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-232">You may also do this in XCode as well:</span></span>

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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="ae6b0-233">Krok 3: Stanovení na nový identifikátor URI s vaše schéma adresy URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="ae6b0-233">Step 3: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="ae6b0-234">Aby se zajistilo, že vrátíme vždy tokeny přihlašovacích údajů pro správnou aplikaci, musíme Ujistěte se, že jsme zpětné volání do vaší aplikace tak, aby operační systém iOS můžete ověřit.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-234">In order to ensure that we always return the credential tokens to the correct application, we need to make sure we call back to your application in a way that the iOS operating system can verify.</span></span> <span data-ttu-id="ae6b0-235">Operační systém iOS sestav ID sady prostředků aplikace volání ho zprostředkovatele aplikací společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-235">The iOS operating system reports to the Microsoft broker applications the Bundle ID of the application calling it.</span></span> <span data-ttu-id="ae6b0-236">To nelze maskování podvodný aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-236">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="ae6b0-237">Proto jsme využít to společně se identifikátor URI aplikace zprostředkovatele k zajištění, že tokeny jsou vráceny správné aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-237">Therefore, we leverage this along with the URI of our broker application to ensure that the tokens are returned to the correct application.</span></span> <span data-ttu-id="ae6b0-238">Je nutné vytvořit tento jedinečný identifikátor URI pro přesměrování oba ve vaší aplikaci a nastavit jako identifikátor URI přesměrování v našem portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-238">We require you to establish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="ae6b0-239">Váš identifikátor URI pro přesměrování musí být ve správném formátu:</span><span class="sxs-lookup"><span data-stu-id="ae6b0-239">Your redirect URI must be in the proper form of:</span></span>

`<app-scheme>://<your.bundle.id>`

<span data-ttu-id="ae6b0-240">například: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="ae6b0-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span></span>

<span data-ttu-id="ae6b0-241">Tento identifikátor URI pro přesměrování musí být zadána v registrace vaší aplikace pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ae6b0-241">This Redirect URI needs to be specified in your app registration using the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="ae6b0-242">Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="ae6b0-242">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a><span data-ttu-id="ae6b0-243">Krok 3a: přidejte identifikátor URI přesměrování vaší aplikace a vývojářů portálu pro podporu ověřování na základě certifikátů</span><span class="sxs-lookup"><span data-stu-id="ae6b0-243">Step 3a: Add a redirect URI in your app and dev portal to support certificate based authentication</span></span>
<span data-ttu-id="ae6b0-244">Pro podporu ověřování na základě certifikátu druhý "msauth" musí být zaregistrovaný ve vaší aplikaci a [portál Azure](https://portal.azure.com/) pro zpracování ověření certifikátu, pokud chcete přidat podporující ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-244">To support cert based authentication a second "msauth"  needs to be registered in your application and the [Azure portal](https://portal.azure.com/) to handle certificate authentication if you wish to add that support in your application.</span></span>

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

<span data-ttu-id="ae6b0-245">například: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="ae6b0-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span></span>

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a><span data-ttu-id="ae6b0-246">Krok 4: iOS9: Přidání konfiguračního parametru do aplikace</span><span class="sxs-lookup"><span data-stu-id="ae6b0-246">Step 4: iOS9: Add a configuration parameter to your app</span></span>
<span data-ttu-id="ae6b0-247">ADAL používá – canOpenURL: Zkontrolujte, zda je na zařízení nainstalován zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-247">ADAL uses –canOpenURL: to check if the broker is installed on the device.</span></span> <span data-ttu-id="ae6b0-248">V systému iOS 9 Apple uzamčené co schémata aplikaci dotázat.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-248">In iOS 9 Apple locked down what schemes an application can query for.</span></span> <span data-ttu-id="ae6b0-249">Budete muset přidat do sekce LSApplicationQueriesSchemes "msauth" vaší `info.plist file`.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-249">You will need to add “msauth” to the LSApplicationQueriesSchemes section of your `info.plist file`.</span></span>

<span data-ttu-id="ae6b0-250"><key>LSApplicationQueriesSchemes</key></span><span class="sxs-lookup"><span data-stu-id="ae6b0-250"><key>LSApplicationQueriesSchemes</key></span></span>

<span data-ttu-id="ae6b0-251"><array><string>msauth</string>
</array></span><span class="sxs-lookup"><span data-stu-id="ae6b0-251"><array> <string>msauth</string>
</array></span></span>

### <a name="youve-configured-sso"></a><span data-ttu-id="ae6b0-252">Jednotné přihlašování jste nakonfigurovali!</span><span class="sxs-lookup"><span data-stu-id="ae6b0-252">You've configured SSO!</span></span>
<span data-ttu-id="ae6b0-253">Nyní Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele, pokud je k dispozici na svém zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae6b0-253">Now the Microsoft Identity SDK will automatically both share credentials across your applications and invoke the broker if it's present on their device.</span></span>

