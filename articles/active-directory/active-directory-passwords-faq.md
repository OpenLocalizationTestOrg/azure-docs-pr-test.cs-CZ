---
title: "Nejčastější dotazy: Azure AD SSPR | Microsoft Docs"
description: "Nejčastější dotazy o hesla pomocí samoobslužné služby Azure AD resetovat"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b3fab99ff9fab5bc67fa70113dc5b06fac775b09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="74e2b-104">Nejčastější dotazy se správou hesel</span><span class="sxs-lookup"><span data-stu-id="74e2b-104">Password management frequently asked questions</span></span>

<span data-ttu-id="74e2b-105">Níže jsou uvedeny některé nejčastější dotazy pro všechny věci týkající se vytvoření nového hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-105">The following are some frequently asked questions for all things related to password reset.</span></span>

<span data-ttu-id="74e2b-106">Pokud máte obecný dotaz týkající se Azure AD a hesla pomocí samoobslužné služby resetování, která není zde zodpovězena, můžete požádat komunitou o pomoc na [fóra Azure Ad](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="74e2b-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask the community for assistance on the [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="74e2b-107">Členové komunity služby zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.</span><span class="sxs-lookup"><span data-stu-id="74e2b-107">Members of the community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="74e2b-108">Tyto nejčastější dotazy je rozdělená do následujících částí:</span><span class="sxs-lookup"><span data-stu-id="74e2b-108">This FAQ is split into the following sections:</span></span>

* [<span data-ttu-id="74e2b-109">**Dotazy o registraci k resetování hesla**</span><span class="sxs-lookup"><span data-stu-id="74e2b-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="74e2b-110">**Dotazy o resetování hesla**</span><span class="sxs-lookup"><span data-stu-id="74e2b-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="74e2b-111">**Dotazy týkající se změna hesla**</span><span class="sxs-lookup"><span data-stu-id="74e2b-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="74e2b-112">**Sestavy dotazy týkající se správou hesel**</span><span class="sxs-lookup"><span data-stu-id="74e2b-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="74e2b-113">**Dotazy týkající se zpětný zápis hesla**</span><span class="sxs-lookup"><span data-stu-id="74e2b-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="74e2b-114">Registrace pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="74e2b-114">Password reset registration</span></span>
* <span data-ttu-id="74e2b-115">**Otázka: je možné mé uživatelé registrovat svá vlastní data resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="74e2b-116">**Odpověď:** Ano, jak dlouho, dokud je povoleno obnovení hesla a udělení licence, mohou přejít na portál registraci pro resetování hesla na adrese http://aka.ms/ssprsetup k registraci jejich informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="74e2b-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go to the Password Reset Registration portal at http://aka.ms/ssprsetup to register their authentication information.</span></span> <span data-ttu-id="74e2b-117">Uživatelé mohou také registrovat přejdete k přístupovému panelu na adrese http://myapps.microsoft.com, kliknutím na kartu profil a kliknutím na registrace pro resetování hesla možnost.</span><span class="sxs-lookup"><span data-stu-id="74e2b-117">Users can also register by going to the access panel at http://myapps.microsoft.com, clicking the profile tab, and clicking the Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="74e2b-118">**Otázka: je možné definovat data resetování hesel jménem Moje uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="74e2b-119">**Odpověď:** Ano, můžete tak učinit službou Azure AD Connect, prostředí PowerShell [portál Azure](https://portal.azure.com), nebo portál pro správu Office.</span><span class="sxs-lookup"><span data-stu-id="74e2b-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, the [Azure portal](https://portal.azure.com), or the Office Admin portal.</span></span> <span data-ttu-id="74e2b-120">Další informace najdete v článku [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="74e2b-120">For more information, see the article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="74e2b-121">**Otázka: je možné synchronizovat data pro bezpečnostní otázky z místně?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="74e2b-122">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="74e2b-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="74e2b-123">**Otázka: je možné zaregistrovat vlastní uživatelé dat tak, že další uživatelé nemohou zobrazit tato data?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="74e2b-124">**Odpověď:** Ano, když uživatelé zaregistrovat dat pomocí registrace portálu pro resetování hesel je uložen do polí privátní ověřování, která viditelné pouze globální správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="74e2b-124">**A:** Yes, when users register data using the Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and the user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="74e2b-125">Pokud **účet správce Azure** zaregistruje jejich číslo telefonu pro ověření ní se importují také do pole mobilní telefon a je viditelný.</span><span class="sxs-lookup"><span data-stu-id="74e2b-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into the mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="74e2b-126">**Otázka: Moji uživatelé mají k registraci, aby mohli používat resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-126">**Q:  Do my users have to be registered before they can use password reset?**</span></span>

  > <span data-ttu-id="74e2b-127">**Odpověď:** Ne, pokud jejich jménem definujete dostatek informací, ověřování, uživatelé nebudou mít k registraci.</span><span class="sxs-lookup"><span data-stu-id="74e2b-127">**A:** No, if you define enough authentication information on their behalf, users do not have to register.</span></span> <span data-ttu-id="74e2b-128">Tak dlouho, dokud máte správně naformátovaná data uložená v odpovídající pole v adresáři, resetování hesla funguje.</span><span class="sxs-lookup"><span data-stu-id="74e2b-128">Password reset works as long as you have properly formatted data stored in the appropriate fields in the directory.</span></span>
  >
  >
* <span data-ttu-id="74e2b-129">**Otázka: je možné synchronizovat nebo nastavit telefon pro ověření, ověření e-mailu nebo alternativní telefon pro ověření pole jménem Moje uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-129">**Q:  Can I synchronize or set the Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="74e2b-130">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="74e2b-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="74e2b-131">**Otázka: jak na portál pro registraci vědět, které možnosti zobrazíte Moji uživatelé?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-131">**Q:  How does the registration portal know which options to show my users?**</span></span>

  > <span data-ttu-id="74e2b-132">**Odpověď:** zobrazí pouze portálu registrace resetování hesel možností, že je povoleno pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="74e2b-132">**A:** The password reset registration portal only shows the options that you have enabled for your users.</span></span> <span data-ttu-id="74e2b-133">Tyto možnosti se nacházejí v části zásady resetování hesel uživateli svého adresáře na kartě konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74e2b-133">These options are found under the User Password Reset Policy section of your directory’s Configure tab.</span></span> <span data-ttu-id="74e2b-134">Například to znamená, že pokud nepovolíte bezpečnostní otázky, pak nebudou se uživatelé moct zaregistrovat pro tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="74e2b-134">For example, this means that if you do not enable security questions, then users are not able to register for that option.</span></span>
  >
  >
* <span data-ttu-id="74e2b-135">**Otázka: když se považuje za uživatele registrované?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-135">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="74e2b-136">**Odpověď:** uživatel se považuje zaregistrovat pro SSPR, když registrace alespoň **několik metod, které jsou nutná k obnovení** , kterou jste nastavili [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="74e2b-136">**A:** A user is considered registered for SSPR when they have registered at least the **Number of methods required to reset** that you have set in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="74e2b-137">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="74e2b-137">Password reset</span></span>
* <span data-ttu-id="74e2b-138">**Otázka: jak dlouho má čekat pro příjem e-mailem, SMS nebo telefonní hovor z resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-138">**Q:  How long should I wait to receive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="74e2b-139">**Odpověď:** e-mailu, zpráv SMS, a telefonních hovorů by měl přicházející do části jednu minutu, normální případem 5-20 sekund.</span><span class="sxs-lookup"><span data-stu-id="74e2b-139">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with the normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="74e2b-140">Pokud tento časový rámec neobdrží oznámení:</span><span class="sxs-lookup"><span data-stu-id="74e2b-140">If you do not receive the notification in this time frame:</span></span>
        > * <span data-ttu-id="74e2b-141">Zkontrolujte své složky s nevyžádanou poštou.</span><span class="sxs-lookup"><span data-stu-id="74e2b-141">Check your junk folder.</span></span>
        > * <span data-ttu-id="74e2b-142">Číslo nebo e-mailu kontaktovaný je ta, kterou byste měli zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="74e2b-142">Check the number or email being contacted is the one you expect.</span></span>
        > * <span data-ttu-id="74e2b-143">Zkontrolujte, zda že je správně naformátován ověřování dat v adresáři.</span><span class="sxs-lookup"><span data-stu-id="74e2b-143">Check the authentication data in the directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="74e2b-144">Příklad: "+ 1 4255551234" nebo "user@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="74e2b-144">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="74e2b-145">**Otázka: jaké jazyky jsou podporovány resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-145">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="74e2b-146">**Odpověď:** heslo resetovat uživatelského rozhraní, zpráv SMS a hlasové hovory jsou lokalizované do stejné jazyky, které jsou podporovány v Office 365.</span><span class="sxs-lookup"><span data-stu-id="74e2b-146">**A:** The password reset UI, SMS messages, and voice calls are localized in the same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="74e2b-147">**Otázka: jaký částí prostředí resetování hesla získat značky po nastavení organizační brandingu v adresáře Moje aktivity karta Konfigurace?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-147">**Q:  What parts of the password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="74e2b-148">**Odpověď:** portálu pro resetování hesla zobrazí logo vaší organizace a umožňuje vám nakonfigurovat kontakt odkaz na vaši správce tak, aby odkazovalo vlastní e-mailu nebo adresa URL.</span><span class="sxs-lookup"><span data-stu-id="74e2b-148">**A:** The password reset portal shows your organizational logo and allows you to configure the Contact your administrator link to point to a custom email or URL.</span></span> <span data-ttu-id="74e2b-149">E-mailu, která se odešlou resetování hesla zahrnuje loga vaší organizace, barvy, název v textu e-mailu a přizpůsobit z názvu.</span><span class="sxs-lookup"><span data-stu-id="74e2b-149">Any email that gets sent by password reset includes your organization’s logo, colors, name in the body of the email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="74e2b-150">**Otázka: jak můžete naučit Moje uživatele o tom, kde přejděte k resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-150">**Q:  How can I educate my users about where to go to reset their passwords?**</span></span>

  > <span data-ttu-id="74e2b-151">**Odpověď:** můžete odeslat uživatelům https://passwordreset.microsoftonline.com přímo, nebo můžete určit, aby mohly klikněte na tlačítko **nemůže získat přístup k účtu odkaz na vaši** nalezen na libovolné pracovní nebo školní přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="74e2b-151">**A:** You can send your users to https://passwordreset.microsoftonline.com directly, or you can instruct them to click the **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="74e2b-152">Tyto odkazy můžete také publikovat na místě, které jsou snadno přístupné pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="74e2b-152">You can also publish these links in a place easily accessible to your users.</span></span>
  >
  >
* <span data-ttu-id="74e2b-153">**Otázka: je možné pomocí této stránky z mobilního zařízení?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-153">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="74e2b-154">**Odpověď:** Ano, tato stránka funguje na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="74e2b-154">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="74e2b-155">**Otázka: podporujete odemykání účty místní služby active directory při resetování hesel uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-155">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="74e2b-156">**Odpověď:** Ano, pokud uživatel může resetovat své heslo a byla nasazena zpětného zápisu hesla pomocí služby Azure AD Connect, tento uživatelský účet je automaticky odemknout se obnovit své heslo.</span><span class="sxs-lookup"><span data-stu-id="74e2b-156">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="74e2b-157">**Otázka: jak můžete integrovat přímo do daného uživatele plochy přihlašování resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-157">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="74e2b-158">**Odpověď:** Pokud jste zákazník Azure AD Premium, můžete nainstalovat Microsoft Identity Manager bez dalších poplatků a nasazení v případě místních řešení resetování hesla ke splnění tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="74e2b-158">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy the on-premises password reset solution to meet this requirement.</span></span>
  >
  >
* <span data-ttu-id="74e2b-159">**Otázka: je možné nastavit různé bezpečnostní otázky pro různá národní prostředí?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-159">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="74e2b-160">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="74e2b-160">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="74e2b-161">**Otázka: jak tolik otázek jsme nakonfigurovat pro možnost ověření bezpečnostních otázek?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-161">**Q:  How many questions can we configure for the Security Questions authentication option?**</span></span>

  > <span data-ttu-id="74e2b-162">**Odpověď:** můžete nakonfigurovat až 20 vlastní bezpečnostních otázek v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="74e2b-162">**A:** You can configure up to 20 custom security questions in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="74e2b-163">**Otázka: jak dlouho může bezpečnostní otázky být?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-163">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="74e2b-164">**Odpověď:** bezpečnostní otázky může být 3 až 200 znaků.</span><span class="sxs-lookup"><span data-stu-id="74e2b-164">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="74e2b-165">**Otázka: jak dlouho může odpovědi na bezpečnostní otázky být?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-165">**Q:  How long may answers to security questions be?**</span></span>

  > <span data-ttu-id="74e2b-166">**Odpověď:** odpovědi může mít délku 3 až 40 znaků.</span><span class="sxs-lookup"><span data-stu-id="74e2b-166">**A:** Answers may be 3 to 40 characters long.</span></span>
  >
  >
* <span data-ttu-id="74e2b-167">**Otázka: je duplicitní odpovědi na bezpečnostní otázky odmítl?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-167">**Q:  Are duplicate answers to security questions rejected?**</span></span>

  > <span data-ttu-id="74e2b-168">**Odpověď:** Ano, jsme odmítnout duplicitní odpovědi na bezpečnostní otázky.</span><span class="sxs-lookup"><span data-stu-id="74e2b-168">**A:** Yes, we reject duplicate answers to security questions.</span></span>
  >
  >
* <span data-ttu-id="74e2b-169">**Otázka: může uživatel na stejné bezpečnostní otázku zaregistrovat více než jednou?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-169">**Q:  May a user register the same security question more than once?**</span></span>

  > <span data-ttu-id="74e2b-170">**Odpověď:** Ne, jakmile se uživatel zaregistruje na určitou otázku, se nemusí zaregistrovat pro tento dotaz ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="74e2b-170">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="74e2b-171">**Otázka: je možné nastavit minimální limit bezpečnostní otázky pro registraci a resetování?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-171">**Q:  Is it possible to set a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="74e2b-172">**Odpověď:** Ano, lze nastavit jeden limit pro registraci a druhý pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="74e2b-172">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="74e2b-173">bezpečnostní otázky 3 až 5 může být potřeba k registraci a 3 až 5 může být nutný pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="74e2b-173">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="74e2b-174">**Otázka: Pokud uživatel má více než maximální počet otázek vyžadovaných k resetování registrované, jak jsou bezpečnostní otázky vybrané během obnovení?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-174">**Q:  If a user has registered more than the maximum number of questions required to reset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="74e2b-175">**Odpověď:** N bezpečnostní otázky jsou náhodně vybrané mimo celkový počet otázek, které uživatel zaregistroval, kde N je **počet otázek vyžadovaných k resetování**.</span><span class="sxs-lookup"><span data-stu-id="74e2b-175">**A:** N security questions are selected at random out of the total number of questions a user has registered for, where N is the **Number of questions required to reset**.</span></span> <span data-ttu-id="74e2b-176">Například pokud má uživatel zaregistrován 5 bezpečnostní otázky, ale jenom 3 jsou nutná k obnovení, 3 5 jsou vybrán náhodně a uvedené v resetování.</span><span class="sxs-lookup"><span data-stu-id="74e2b-176">For example, if a user has 5 security questions registered, but only 3 are required to reset, 3 of the 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="74e2b-177">Pokud uživatel získá nesprávné odpovědi na otázky, aby se zabránilo ražením otázku opakovat procesu výběru.</span><span class="sxs-lookup"><span data-stu-id="74e2b-177">If the user gets the answers to the questions wrong, the selection process reoccurs to prevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="74e2b-178">**Otázka: můžete zabránit uživatelům v pokusu o mnohokrát v krátkého časového období pro vytvoření nového hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-178">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="74e2b-179">**Odpověď:** Ano, jsou funkce zabezpečení, které jsou součástí pro ochranu před zneužitím pro vytvoření nového hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-179">**A:** Yes, there are security features built into password reset to protect from misuse.</span></span> <span data-ttu-id="74e2b-180">Uživatelé mohou zkuste jenom 5 resetování pokusů o zadání hesla v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="74e2b-180">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="74e2b-181">Uživatelé mohou pouze pokusí se ověřit telefonní číslo 5krát v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="74e2b-181">Users may only try to validate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="74e2b-182">Uživatelé mohou pouze pokusí metoda ověření jednotného 5krát v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="74e2b-182">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="74e2b-183">**Otázka: po tom, jak dlouho jsou e-mailu a SMS jednorázovým heslem platné?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-183">**Q:  For how long are the email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="74e2b-184">**Odpověď:** dobu platnosti relace pro resetování hesla je 105 minut.</span><span class="sxs-lookup"><span data-stu-id="74e2b-184">**A:** The session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="74e2b-185">Od začátku operace resetování hesla, uživatel má 105 minut resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="74e2b-185">From the beginning of the password reset operation, the user has 105 minutes to reset their password.</span></span> <span data-ttu-id="74e2b-186">E-mailu a SMS jednorázové heslo jsou neplatné po vypršení tohoto časového období.</span><span class="sxs-lookup"><span data-stu-id="74e2b-186">The email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="74e2b-187">Změna hesla</span><span class="sxs-lookup"><span data-stu-id="74e2b-187">Password change</span></span>
* <span data-ttu-id="74e2b-188">**Otázka: kde by měli Moji uživatelé přejít ke změně hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-188">**Q:  Where should my users go to change their passwords?**</span></span>

  > <span data-ttu-id="74e2b-189">**A:** uživatelé mohou změnit své heslo kdekoli uvidí jejich profilový obrázek nebo ikonu (jako v pravém horním rohu jejich [Office 365](https://portal.office.com) nebo [přístupový Panel](https://myapps.microsoft.com) dojde.</span><span class="sxs-lookup"><span data-stu-id="74e2b-189">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in the upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="74e2b-190">Uživatelé mohou změnit své heslo z [stránka přístupového panelu profil](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="74e2b-190">Users may change their passwords from the [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="74e2b-191">Uživatelé mohou také vyzváni ke změně hesla na přihlašovací obrazovce Azure AD automaticky, pokud vypršela platnost hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-191">Users may also be asked to change their passwords automatically at the Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="74e2b-192">Nakonec mohou uživatelé přejdou [portál změn hesel služby Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) přímo Pokud chtějí změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="74e2b-192">Finally, users may navigate to the [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish to change their passwords.</span></span>
  >
  >
* <span data-ttu-id="74e2b-193">**Otázka: je možné se svým uživatelům a upozornění na portálu Office, když vyprší platnost hesla pro místní?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-193">**Q:  Can my users be notified in the Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="74e2b-194">**Odpověď:** to je možné ještě dnes Pokud používáte služby AD FS podle pokynů tady: [odesílání deklarace zásady hesel se službou AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="74e2b-194">**A:** This is possible today if you are using ADFS by following the instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="74e2b-195">Pokud používáte synchronizaci hodnoty hash hesla, to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="74e2b-195">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="74e2b-196">Je to proto, že jsme není synchronizována zásady hesel z místní, takže není možné, že nám odeslat oznámení o vypršení platnosti do prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="74e2b-196">This is because we do not sync password policies from on-premises, so it is not possible for us to post expiry notifications to cloud experiences.</span></span> <span data-ttu-id="74e2b-197">V obou případech je také možné [upozorněte uživatele, jejichž hesla se vyprší pomocí prostředí PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="74e2b-197">In either case, it is also possible to [notify users whose passwords are about to expire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="74e2b-198">Sestavy správy hesel</span><span class="sxs-lookup"><span data-stu-id="74e2b-198">Password management reports</span></span>
* <span data-ttu-id="74e2b-199">**Otázka: jak dlouho trvá pro data objeví na sestavy správy heslo?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-199">**Q:  How long does it take for data to show up on the password management reports?**</span></span>

  > <span data-ttu-id="74e2b-200">**Odpověď:** Data by se zobrazit na sestav správy hesel v rámci 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="74e2b-200">**A:** Data should appear on the password management reports within 5-10 minutes.</span></span> <span data-ttu-id="74e2b-201">Ho některých případech může trvat jednu hodinu, než se objeví.</span><span class="sxs-lookup"><span data-stu-id="74e2b-201">It some instances it may take up to an hour to appear.</span></span>
  >
  >
* <span data-ttu-id="74e2b-202">**Otázka: jak můžete filtrovat sestavy správy heslo?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-202">**Q:  How can I filter the password management reports?**</span></span>

  > <span data-ttu-id="74e2b-203">**Odpověď:** sestav správy hesel můžete filtrovat kliknutím malé lupy extrémně napravo od popisky sloupců, do horní části sestavy.</span><span class="sxs-lookup"><span data-stu-id="74e2b-203">**A:** You can filter the password management reports by clicking the small magnifying glass to the extreme right of the column labels, near the top of the report.</span></span> <span data-ttu-id="74e2b-204">Pokud chcete provádět bohatší filtrování, můžete si stáhnout sestavu, aby se v aplikaci excel a vytvoření kontingenční tabulky.</span><span class="sxs-lookup"><span data-stu-id="74e2b-204">If you want to do richer filtering, you can download the report to excel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="74e2b-205">**Otázka: co je maximální počet událostí, které jsou uloženy v sestavy správy heslo?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-205">**Q: What is the maximum number of events are stored in the password management reports?**</span></span>

  > <span data-ttu-id="74e2b-206">**Odpověď:** až než 75 000 resetování nebo heslo registraci k resetování hesla události jsou uložené v sestav správy hesel, pokrývání uzlů zpět až 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="74e2b-206">**A:** Up to 75,000 password reset or password reset registration events are stored in the password management reports, spanning back up to 30 days.</span></span>  <span data-ttu-id="74e2b-207">Pracujeme na rozbalte tento počet na obsahovat další události.</span><span class="sxs-lookup"><span data-stu-id="74e2b-207">We are working to expand this number to include more events.</span></span>
  >
  >
* <span data-ttu-id="74e2b-208">**Otázka: jak daleko zpět sestav správy hesel přejděte?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-208">**Q:  How far back do the password management reports go?**</span></span>

  > <span data-ttu-id="74e2b-209">**Odpověď:** Správa hesel sestavy zobrazit operace, ke kterým došlo během posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="74e2b-209">**A:** The password management reports show operations occurring within the last 30 days.</span></span> <span data-ttu-id="74e2b-210">Teď Pokud budete potřebovat k archivaci tato data můžete stáhnout sestavy pravidelně a uložit je do samostatných umístění.</span><span class="sxs-lookup"><span data-stu-id="74e2b-210">For now, if you need to archive this data, you can download the reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="74e2b-211">**Otázka: je maximální počet řádků, které se mohou objevit v sestavy správy heslo?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-211">**Q:  Is there a maximum number of rows that can appear on the password management reports?**</span></span>

  > <span data-ttu-id="74e2b-212">**Odpověď:** Ano, nesmí být delší než 75 000 řádků mohou být na buď sestavy správy hesel, jestli se zobrazí v uživatelském rozhraní nebo stahování.</span><span class="sxs-lookup"><span data-stu-id="74e2b-212">**A:** Yes, a maximum of 75,000 rows may appear on either of the password management reports, whether they are being shown in the UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="74e2b-213">**Otázka: je rozhraní API pro přístup k resetování hesla nebo registrační data pro vytváření sestav?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-213">**Q:  Is there an API to access the password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="74e2b-214">**Odpověď:** Ano, naleznete v následující dokumentaci se dozvíte, jak přistupovat k resetování hesla, vytváření sestav datového proudu.</span><span class="sxs-lookup"><span data-stu-id="74e2b-214">**A:** Yes, see the following documentation to learn how you can access the password reset reporting data stream.</span></span>  <span data-ttu-id="74e2b-215">[Další informace o přístupu k resetování hesla události vytváření sestav prostřednictvím kódu programu](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="74e2b-215">[Learn how to access password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="74e2b-216">Zpětný zápis hesla</span><span class="sxs-lookup"><span data-stu-id="74e2b-216">Password writeback</span></span>
* <span data-ttu-id="74e2b-217">**Otázka: jak zpětný zápis hesla funguje na pozadí?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-217">**Q:  How does password writeback work behind the scenes?**</span></span>

  > <span data-ttu-id="74e2b-218">**Odpověď:** najdete v části [jak zpětný zápis hesla funguje](active-directory-passwords-writeback.md) pro vysvětlení, co se stane, když zapnete zpětný zápis hesla a jak se data proudí prostřednictvím systému zpět do místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="74e2b-218">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through the system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="74e2b-219">**Otázka: jak dlouho zpětný zápis hesla trvá fungovat?  Je k dispozici ke zpoždění synchronizace jako se synchronizací hodnoty hash hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-219">**Q:  How long does password writeback take to work?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="74e2b-220">**Odpověď:** zpětný zápis hesla je rychlé.</span><span class="sxs-lookup"><span data-stu-id="74e2b-220">**A:** Password writeback is instant.</span></span> <span data-ttu-id="74e2b-221">Je synchronní kanál, který funguje zásadně jinak než synchronizaci hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-221">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="74e2b-222">Zpětný zápis hesla umožňuje uživatelům získat v reálném čase zpětnou vazbu o úspěšnosti jejich resetování hesla nebo změňte operaci.</span><span class="sxs-lookup"><span data-stu-id="74e2b-222">Password writeback allows users to get real-time feedback about the success of their password reset or change operation.</span></span> <span data-ttu-id="74e2b-223">Průměrná doba pro úspěšné zpětný zápis hesla je v části 500 ms.</span><span class="sxs-lookup"><span data-stu-id="74e2b-223">The average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="74e2b-224">**Otázka: Pokud Můj účet místní je, jak je můj účet nebo přístup do cloudu zneužitím?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-224">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="74e2b-225">**Odpověď:** Pokud vaše místní ID je zakázaná, cloudu ID nebo přístup budou rovněž zakázány na další interval synchronizace prostřednictvím AAD Connect výchozí byt je to každých 30 minut.</span><span class="sxs-lookup"><span data-stu-id="74e2b-225">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at the next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="74e2b-226">**Otázka: Pokud je omezené Můj účet místní zásady hesel místní služby Active Directory, SSPR orientují tuto zásadu po změně hesla?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-226">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change the password?**</span></span>

  > <span data-ttu-id="74e2b-227">**Odpověď:** Ano, spoléhá na SSPR a dodržuje místní zásady hesel služby AD, včetně typické zásady hesel služby AD domény, stejně jako jakoukoli definované zásady podrobné heslo pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="74e2b-227">**A:** Yes, SSPR relies on and abides by the on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted to a given user.</span></span>
  >
  >
* <span data-ttu-id="74e2b-228">**Otázka: jaký typy účtů zpětný zápis hesla funguje pro?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-228">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="74e2b-229">**Odpověď:** zpětný zápis hesla funguje pro federovaný a synchronizuje hodnoty Hash hesla uživatele.</span><span class="sxs-lookup"><span data-stu-id="74e2b-229">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="74e2b-230">**Otázka: zpětný zápis hesla vynutit zásady hesel Moje doména?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-230">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="74e2b-231">**Odpověď:** Ano, zpětný zápis hesla vynucuje stáří hesla, historie, složitost, filtrů a další omezení, můžete mohou zavést na hesla v místní doméně.</span><span class="sxs-lookup"><span data-stu-id="74e2b-231">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="74e2b-232">**Otázka: je zpětný zápis hesla zabezpečení?  Jak se může být jistí, že nebude získat hacker I?**</span><span class="sxs-lookup"><span data-stu-id="74e2b-232">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="74e2b-233">**Odpověď:** Ano, jsou zabezpečené zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-233">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="74e2b-234">Číst informace o čtyři vrstvy zabezpečení implementované službu zpětný zápis hesla, podívejte se [model zabezpečení zpětný zápis hesla](active-directory-passwords-writeback.md#password-writeback-security-model) část v tom, jak zpětný zápis hesla funguje.</span><span class="sxs-lookup"><span data-stu-id="74e2b-234">To read more about the four layers of security implemented by the password writeback service, check out the [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="74e2b-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74e2b-235">Next steps</span></span>

<span data-ttu-id="74e2b-236">Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="74e2b-236">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="74e2b-237">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74e2b-237">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="74e2b-238">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74e2b-238">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="74e2b-239">[**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.</span><span class="sxs-lookup"><span data-stu-id="74e2b-239">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="74e2b-240">[**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="74e2b-240">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="74e2b-241">[**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="74e2b-241">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="74e2b-242">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-242">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="74e2b-243">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74e2b-243">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="74e2b-244">[**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.</span><span class="sxs-lookup"><span data-stu-id="74e2b-244">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="74e2b-245">[**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="74e2b-245">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="74e2b-246">[**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="74e2b-246">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>
