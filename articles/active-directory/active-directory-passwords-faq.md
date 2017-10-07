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
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="091fe-104">Nejčastější dotazy se správou hesel</span><span class="sxs-lookup"><span data-stu-id="091fe-104">Password management frequently asked questions</span></span>

<span data-ttu-id="091fe-105">Hello následující jsou některé nejčastější dotazy pro všechny věci týkající se toopassword resetovat.</span><span class="sxs-lookup"><span data-stu-id="091fe-105">hello following are some frequently asked questions for all things related toopassword reset.</span></span>

<span data-ttu-id="091fe-106">Pokud máte obecný dotaz týkající se Azure AD a samoobslužné služby heslo resetovat, která není zde zodpovězena, můžete požádat hello komunity pro pomoc na hello [fóra Azure Ad](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="091fe-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask hello community for assistance on hello [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="091fe-107">Členové komunity hello zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.</span><span class="sxs-lookup"><span data-stu-id="091fe-107">Members of hello community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="091fe-108">Tyto nejčastější dotazy se rozdělí do hello následující části:</span><span class="sxs-lookup"><span data-stu-id="091fe-108">This FAQ is split into hello following sections:</span></span>

* [<span data-ttu-id="091fe-109">**Dotazy o registraci k resetování hesla**</span><span class="sxs-lookup"><span data-stu-id="091fe-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="091fe-110">**Dotazy o resetování hesla**</span><span class="sxs-lookup"><span data-stu-id="091fe-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="091fe-111">**Dotazy týkající se změna hesla**</span><span class="sxs-lookup"><span data-stu-id="091fe-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="091fe-112">**Sestavy dotazy týkající se správou hesel**</span><span class="sxs-lookup"><span data-stu-id="091fe-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="091fe-113">**Dotazy týkající se zpětný zápis hesla**</span><span class="sxs-lookup"><span data-stu-id="091fe-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="091fe-114">Registrace pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="091fe-114">Password reset registration</span></span>
* <span data-ttu-id="091fe-115">**Otázka: je možné mé uživatelé registrovat svá vlastní data resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="091fe-116">**Odpověď:** Ano, jak dlouho, dokud je povoleno obnovení hesla a udělení licence, mohou přejít portálu toohello registraci pro resetování hesla na http://aka.ms/ssprsetup tooregister své informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="091fe-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go toohello Password Reset Registration portal at http://aka.ms/ssprsetup tooregister their authentication information.</span></span> <span data-ttu-id="091fe-117">Uživatelé mohou také registrovat pomocí budete toohello přístupového panelu v http://myapps.microsoft.com, klikněte na kartu profil hello a příkaz hello registrace pro resetování hesla možnost.</span><span class="sxs-lookup"><span data-stu-id="091fe-117">Users can also register by going toohello access panel at http://myapps.microsoft.com, clicking hello profile tab, and clicking hello Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="091fe-118">**Otázka: je možné definovat data resetování hesel jménem Moje uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="091fe-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="091fe-119">**Odpověď:** Ano, můžete tak učinit pomocí Azure AD Connect, prostředí PowerShell, hello [portál Azure](https://portal.azure.com), nebo portál pro správu Office hello.</span><span class="sxs-lookup"><span data-stu-id="091fe-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, hello [Azure portal](https://portal.azure.com), or hello Office Admin portal.</span></span> <span data-ttu-id="091fe-120">Další informace najdete v článku hello [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="091fe-120">For more information, see hello article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="091fe-121">**Otázka: je možné synchronizovat data pro bezpečnostní otázky z místně?**</span><span class="sxs-lookup"><span data-stu-id="091fe-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="091fe-122">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="091fe-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="091fe-123">**Otázka: je možné zaregistrovat vlastní uživatelé dat tak, že další uživatelé nemohou zobrazit tato data?**</span><span class="sxs-lookup"><span data-stu-id="091fe-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="091fe-124">**Odpověď:** Ano, když uživatelé zaregistrovat dat pomocí hello registrace portálu pro resetování hesel je uložen do polí privátní ověřování, která viditelné pouze globální správci a hello uživatelem.</span><span class="sxs-lookup"><span data-stu-id="091fe-124">**A:** Yes, when users register data using hello Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and hello user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="091fe-125">Pokud **účet správce Azure** zaregistruje jejich číslo telefonu pro ověření ní se importují také do pole hello mobilní telefon a je viditelný.</span><span class="sxs-lookup"><span data-stu-id="091fe-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into hello mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="091fe-126">**Otázka: Moji uživatelé mají toobe zaregistrován před použitím resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-126">**Q:  Do my users have toobe registered before they can use password reset?**</span></span>

  > <span data-ttu-id="091fe-127">**Odpověď:** Ne, pokud jejich jménem definujete dostatek informací, ověřování, uživatelé nebudou mít tooregister.</span><span class="sxs-lookup"><span data-stu-id="091fe-127">**A:** No, if you define enough authentication information on their behalf, users do not have tooregister.</span></span> <span data-ttu-id="091fe-128">Tak dlouho, dokud máte správně naformátovaná data uložená v hello odpovídající pole v adresáři hello resetování hesla funguje.</span><span class="sxs-lookup"><span data-stu-id="091fe-128">Password reset works as long as you have properly formatted data stored in hello appropriate fields in hello directory.</span></span>
  >
  >
* <span data-ttu-id="091fe-129">**Otázka: je možné synchronizovat nebo nastavte pole Telefon pro ověření, ověření e-mailu nebo alternativní telefon pro ověření hello jménem Moje uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="091fe-129">**Q:  Can I synchronize or set hello Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="091fe-130">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="091fe-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="091fe-131">**Otázka: jak portálu pro registraci hello vědět, jaké možnosti tooshow Moji uživatelé?**</span><span class="sxs-lookup"><span data-stu-id="091fe-131">**Q:  How does hello registration portal know which options tooshow my users?**</span></span>

  > <span data-ttu-id="091fe-132">**Odpověď:** resetování hesla hello portálu pro registraci jen ukazuje hello možnosti, které jste povolili pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="091fe-132">**A:** hello password reset registration portal only shows hello options that you have enabled for your users.</span></span> <span data-ttu-id="091fe-133">Tyto možnosti se nacházejí v rámci hello zásady resetování hesel uživateli část svého adresáře na kartě konfigurace. Například to znamená, že pokud nepovolíte bezpečnostní otázky, pak uživatelé nebudou moct tooregister pro tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="091fe-133">These options are found under hello User Password Reset Policy section of your directory’s Configure tab. For example, this means that if you do not enable security questions, then users are not able tooregister for that option.</span></span>
  >
  >
* <span data-ttu-id="091fe-134">**Otázka: když se považuje za uživatele registrované?**</span><span class="sxs-lookup"><span data-stu-id="091fe-134">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="091fe-135">**A:** uživatel se považuje zaregistrovat pro SSPR, když registrace alespoň hello **počet požadovaných tooreset metody** , kterou jste nastavili v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="091fe-135">**A:** A user is considered registered for SSPR when they have registered at least hello **Number of methods required tooreset** that you have set in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="091fe-136">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="091fe-136">Password reset</span></span>
* <span data-ttu-id="091fe-137">**Otázka: jak dlouho má čekat tooreceive e-mailem, SMS nebo telefonní hovor z resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-137">**Q:  How long should I wait tooreceive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="091fe-138">**Odpověď:** e-mailu, zpráv SMS, a telefonní hovory by měl přicházející do v části jedna minuta, 5-20 sekund normální případem hello.</span><span class="sxs-lookup"><span data-stu-id="091fe-138">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with hello normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="091fe-139">Pokud tento časový rámec neobdrží oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="091fe-139">If you do not receive hello notification in this time frame:</span></span>
        > * <span data-ttu-id="091fe-140">Zkontrolujte své složky s nevyžádanou poštou.</span><span class="sxs-lookup"><span data-stu-id="091fe-140">Check your junk folder.</span></span>
        > * <span data-ttu-id="091fe-141">Kontrola hello číslo nebo e-mailu kontaktovaný je hello jeden, které očekáváte.</span><span class="sxs-lookup"><span data-stu-id="091fe-141">Check hello number or email being contacted is hello one you expect.</span></span>
        > * <span data-ttu-id="091fe-142">Zkontrolujte, zda je správně naformátován hello ověřování dat v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="091fe-142">Check hello authentication data in hello directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="091fe-143">Příklad: "+ 1 4255551234" nebo "user@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="091fe-143">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="091fe-144">**Otázka: jaké jazyky jsou podporovány resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-144">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="091fe-145">**Odpověď:** hello resetování hesla uživatelského rozhraní, zpráv SMS a hlasové hovory jsou lokalizované do hello stejné jazyky, které jsou podporovány v Office 365.</span><span class="sxs-lookup"><span data-stu-id="091fe-145">**A:** hello password reset UI, SMS messages, and voice calls are localized in hello same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="091fe-146">**Otázka: co částí prostředí resetování hesla hello získat značky po nastavení organizační brandingu v adresáře Moje aktivity karta Konfigurace?**</span><span class="sxs-lookup"><span data-stu-id="091fe-146">**Q:  What parts of hello password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="091fe-147">**Odpověď:** portálu pro resetování hesel hello zobrazuje logo vaší organizace a umožňuje vám tooconfigure hello obraťte se na správce odkaz toopoint tooa vlastní e-mailu nebo adresa URL.</span><span class="sxs-lookup"><span data-stu-id="091fe-147">**A:** hello password reset portal shows your organizational logo and allows you tooconfigure hello Contact your administrator link toopoint tooa custom email or URL.</span></span> <span data-ttu-id="091fe-148">E-mailu, která se odešlou resetování hesla zahrnuje loga vaší organizace, barvy, název v textu hello hello e-mailů a přizpůsobit z názvu.</span><span class="sxs-lookup"><span data-stu-id="091fe-148">Any email that gets sent by password reset includes your organization’s logo, colors, name in hello body of hello email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="091fe-149">**Otázka: jak můžete naučit Moje uživatele o tom, kde toogo tooreset hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-149">**Q:  How can I educate my users about where toogo tooreset their passwords?**</span></span>

  > <span data-ttu-id="091fe-150">**Odpověď:** toohttps://passwordreset.microsoftonline.com vaši uživatelé můžete odeslat přímo, nebo můžete určit, aby je tooclick hello **nemůže získat přístup k účtu odkaz na vaši** nalezen na libovolné pracovní nebo školní přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="091fe-150">**A:** You can send your users toohttps://passwordreset.microsoftonline.com directly, or you can instruct them tooclick hello **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="091fe-151">Tyto odkazy můžete také publikovat v místě snadno dostupný tooyour uživatelů.</span><span class="sxs-lookup"><span data-stu-id="091fe-151">You can also publish these links in a place easily accessible tooyour users.</span></span>
  >
  >
* <span data-ttu-id="091fe-152">**Otázka: je možné pomocí této stránky z mobilního zařízení?**</span><span class="sxs-lookup"><span data-stu-id="091fe-152">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="091fe-153">**Odpověď:** Ano, tato stránka funguje na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="091fe-153">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="091fe-154">**Otázka: podporujete odemykání účty místní služby active directory při resetování hesel uživatelů?**</span><span class="sxs-lookup"><span data-stu-id="091fe-154">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="091fe-155">**Odpověď:** Ano, pokud uživatel může resetovat své heslo a byla nasazena zpětného zápisu hesla pomocí služby Azure AD Connect, tento uživatelský účet je automaticky odemknout se obnovit své heslo.</span><span class="sxs-lookup"><span data-stu-id="091fe-155">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="091fe-156">**Otázka: jak můžete integrovat přímo do daného uživatele plochy přihlašování resetování hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-156">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="091fe-157">**Odpověď:** Pokud jste zákazník Azure AD Premium, můžete nainstalovat Microsoft Identity Manager bez dalších poplatků a nasadit hello místní heslo resetovat řešení toomeet tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="091fe-157">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy hello on-premises password reset solution toomeet this requirement.</span></span>
  >
  >
* <span data-ttu-id="091fe-158">**Otázka: je možné nastavit různé bezpečnostní otázky pro různá národní prostředí?**</span><span class="sxs-lookup"><span data-stu-id="091fe-158">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="091fe-159">**Odpověď:** to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="091fe-159">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="091fe-160">**Otázka: jak tolik otázek jsme nakonfigurovat pro možnost ověření bezpečnostních otázek hello?**</span><span class="sxs-lookup"><span data-stu-id="091fe-160">**Q:  How many questions can we configure for hello Security Questions authentication option?**</span></span>

  > <span data-ttu-id="091fe-161">**Odpověď:** můžete nakonfigurovat až too20 vlastní bezpečnostních otázek v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="091fe-161">**A:** You can configure up too20 custom security questions in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="091fe-162">**Otázka: jak dlouho může bezpečnostní otázky být?**</span><span class="sxs-lookup"><span data-stu-id="091fe-162">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="091fe-163">**Odpověď:** bezpečnostní otázky může být 3 až 200 znaků.</span><span class="sxs-lookup"><span data-stu-id="091fe-163">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="091fe-164">**Otázka: jak dlouho může odpovědi toosecurity otázky být?**</span><span class="sxs-lookup"><span data-stu-id="091fe-164">**Q:  How long may answers toosecurity questions be?**</span></span>

  > <span data-ttu-id="091fe-165">**Odpověď:** odpovědi může mít 3 too40 znaků.</span><span class="sxs-lookup"><span data-stu-id="091fe-165">**A:** Answers may be 3 too40 characters long.</span></span>
  >
  >
* <span data-ttu-id="091fe-166">**Otázka: jsou duplicitní odpovědi toosecurity otázky odmítnuta?**</span><span class="sxs-lookup"><span data-stu-id="091fe-166">**Q:  Are duplicate answers toosecurity questions rejected?**</span></span>

  > <span data-ttu-id="091fe-167">**Odpověď:** Ano, jsme odmítnout otázky toosecurity duplicitní odpovědi.</span><span class="sxs-lookup"><span data-stu-id="091fe-167">**A:** Yes, we reject duplicate answers toosecurity questions.</span></span>
  >
  >
* <span data-ttu-id="091fe-168">**Otázka: může uživatel zaregistrovat stejné bezpečnostní otázku hello více než jednou?**</span><span class="sxs-lookup"><span data-stu-id="091fe-168">**Q:  May a user register hello same security question more than once?**</span></span>

  > <span data-ttu-id="091fe-169">**Odpověď:** Ne, jakmile se uživatel zaregistruje na určitou otázku, se nemusí zaregistrovat pro tento dotaz ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="091fe-169">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="091fe-170">**Otázka: je možné tooset minimální limit bezpečnostní otázky pro registraci a resetování?**</span><span class="sxs-lookup"><span data-stu-id="091fe-170">**Q:  Is it possible tooset a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="091fe-171">**Odpověď:** Ano, lze nastavit jeden limit pro registraci a druhý pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="091fe-171">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="091fe-172">bezpečnostní otázky 3 až 5 může být potřeba k registraci a 3 až 5 může být nutný pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="091fe-172">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="091fe-173">**Otázka: Pokud uživatel má více než maximální počet otázek požadované tooreset hello registrované, jak jsou bezpečnostní otázky vybrané během obnovení?**</span><span class="sxs-lookup"><span data-stu-id="091fe-173">**Q:  If a user has registered more than hello maximum number of questions required tooreset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="091fe-174">**Odpověď:** N bezpečnostní otázky jsou náhodně vybírány mimo hello celkový počet otázek a uživatel zaregistroval, kde N je hello **počet otázek požadované tooreset**.</span><span class="sxs-lookup"><span data-stu-id="091fe-174">**A:** N security questions are selected at random out of hello total number of questions a user has registered for, where N is hello **Number of questions required tooreset**.</span></span> <span data-ttu-id="091fe-175">Například pokud má uživatel zaregistrován 5 bezpečnostní otázky, ale jenom 3 jsou požadované tooreset, 3 hello 5 jsou vybrán náhodně a uvedené v resetování.</span><span class="sxs-lookup"><span data-stu-id="091fe-175">For example, if a user has 5 security questions registered, but only 3 are required tooreset, 3 of hello 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="091fe-176">Pokud hello uživatel získá hello odpovědi toohello otázky nesprávný, procesu výběru hello vyskytovat i nadále ražením tooprevent otázku.</span><span class="sxs-lookup"><span data-stu-id="091fe-176">If hello user gets hello answers toohello questions wrong, hello selection process reoccurs tooprevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="091fe-177">**Otázka: můžete zabránit uživatelům v pokusu o mnohokrát v krátkého časového období pro vytvoření nového hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-177">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="091fe-178">**Odpověď:** Ano, jsou součástí tooprotect resetování hesla před zneužitím funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="091fe-178">**A:** Yes, there are security features built into password reset tooprotect from misuse.</span></span> <span data-ttu-id="091fe-179">Uživatelé mohou zkuste jenom 5 resetování pokusů o zadání hesla v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="091fe-179">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="091fe-180">Uživatelé pouze mohou zkuste toovalidate telefonní číslo 5krát v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="091fe-180">Users may only try toovalidate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="091fe-181">Uživatelé mohou pouze pokusí metoda ověření jednotného 5krát v rámci jednu hodinu před uzamknutí 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="091fe-181">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="091fe-182">**Otázka: jak dlouho měly hello e-mailu a SMS jednorázové heslo platné?**</span><span class="sxs-lookup"><span data-stu-id="091fe-182">**Q:  For how long are hello email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="091fe-183">**Odpověď:** hello dobu platnosti relace pro resetování hesla je 105 minut.</span><span class="sxs-lookup"><span data-stu-id="091fe-183">**A:** hello session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="091fe-184">Od začátku hello hello resetování hesla operace, má uživatel hello 105 minut tooreset své heslo.</span><span class="sxs-lookup"><span data-stu-id="091fe-184">From hello beginning of hello password reset operation, hello user has 105 minutes tooreset their password.</span></span> <span data-ttu-id="091fe-185">Hello e-mailu a SMS jednorázové heslo jsou neplatné po vypršení tohoto časového období.</span><span class="sxs-lookup"><span data-stu-id="091fe-185">hello email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="091fe-186">Změna hesla</span><span class="sxs-lookup"><span data-stu-id="091fe-186">Password change</span></span>
* <span data-ttu-id="091fe-187">**Otázka: kde by měl mé uživatelé toochange hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-187">**Q:  Where should my users go toochange their passwords?**</span></span>

  > <span data-ttu-id="091fe-188">**A:** uživatelé mohou změnit své heslo kdekoli uvidí jejich profilový obrázek nebo ikonu (stejně jako v nástroji hello pravého horního rohu jejich [Office 365](https://portal.office.com) nebo [přístupový Panel](https://myapps.microsoft.com) dojde.</span><span class="sxs-lookup"><span data-stu-id="091fe-188">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in hello upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="091fe-189">Uživatelé mohou změnit své heslo z hello [stránka přístupového panelu profil](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="091fe-189">Users may change their passwords from hello [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="091fe-190">Uživatelé mohou být také vyzváni toochange hesla na přihlašovací obrazovce hello Azure AD automaticky, pokud vypršela platnost hesla.</span><span class="sxs-lookup"><span data-stu-id="091fe-190">Users may also be asked toochange their passwords automatically at hello Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="091fe-191">Nakonec mohou uživatelé přejdou toohello [portál změn hesel služby Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) přímo Pokud si přejí toochange jejich hesla.</span><span class="sxs-lookup"><span data-stu-id="091fe-191">Finally, users may navigate toohello [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish toochange their passwords.</span></span>
  >
  >
* <span data-ttu-id="091fe-192">**Otázka: je možné se svým uživatelům a upozornění v hello portál Office, když vyprší platnost hesla pro místní?**</span><span class="sxs-lookup"><span data-stu-id="091fe-192">**Q:  Can my users be notified in hello Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="091fe-193">**Odpověď:** to je možné ještě dnes Pokud používáte služby AD FS podle pokynů hello zde: [odesílání deklarace zásady hesel se službou AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="091fe-193">**A:** This is possible today if you are using ADFS by following hello instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="091fe-194">Pokud používáte synchronizaci hodnoty hash hesla, to není možné ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="091fe-194">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="091fe-195">Důvodem je, že jsme není synchronizována zásady hesel z místní, takže není možné, že nám toopost vypršení platnosti oznámení toocloud dojde.</span><span class="sxs-lookup"><span data-stu-id="091fe-195">This is because we do not sync password policies from on-premises, so it is not possible for us toopost expiry notifications toocloud experiences.</span></span> <span data-ttu-id="091fe-196">V obou případech je také možné příliš[upozorněte uživatele, jejichž hesla se o tooexpire pomocí prostředí PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="091fe-196">In either case, it is also possible too[notify users whose passwords are about tooexpire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="091fe-197">Sestavy správy hesel</span><span class="sxs-lookup"><span data-stu-id="091fe-197">Password management reports</span></span>
* <span data-ttu-id="091fe-198">**Otázka: jak dlouho trvá pro data tooshow až na sestavy správy hello heslo?**</span><span class="sxs-lookup"><span data-stu-id="091fe-198">**Q:  How long does it take for data tooshow up on hello password management reports?**</span></span>

  > <span data-ttu-id="091fe-199">**Odpověď:** Data by se zobrazit na sestavy správy hello heslo v rámci 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="091fe-199">**A:** Data should appear on hello password management reports within 5-10 minutes.</span></span> <span data-ttu-id="091fe-200">Ho některých případech to může trvat až hodinu tooappear tooan.</span><span class="sxs-lookup"><span data-stu-id="091fe-200">It some instances it may take up tooan hour tooappear.</span></span>
  >
  >
* <span data-ttu-id="091fe-201">**Otázka: jak můžete filtrovat sestavy správy hello heslo?**</span><span class="sxs-lookup"><span data-stu-id="091fe-201">**Q:  How can I filter hello password management reports?**</span></span>

  > <span data-ttu-id="091fe-202">**Odpověď:** hello heslo správy sestavy můžete filtrovat kliknutím hello lupy malé toohello extrémně napravo od hello popisky sloupců, v horní hello hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="091fe-202">**A:** You can filter hello password management reports by clicking hello small magnifying glass toohello extreme right of hello column labels, near hello top of hello report.</span></span> <span data-ttu-id="091fe-203">Pokud chcete toodo bohatší filtrování, můžete stáhnout tooexcel hello sestavy a vytvoření kontingenční tabulky.</span><span class="sxs-lookup"><span data-stu-id="091fe-203">If you want toodo richer filtering, you can download hello report tooexcel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="091fe-204">**Otázka: co je hello maximální počet událostí, které jsou uloženy v sestavy správy hello heslo?**</span><span class="sxs-lookup"><span data-stu-id="091fe-204">**Q: What is hello maximum number of events are stored in hello password management reports?**</span></span>

  > <span data-ttu-id="091fe-205">**Odpověď:** až too75, 000 heslo resetovat heslo resetovat registrace události nebo ukládají do sestavy správy hello heslo pokrývání uzlů zálohování too30 dnů.</span><span class="sxs-lookup"><span data-stu-id="091fe-205">**A:** Up too75,000 password reset or password reset registration events are stored in hello password management reports, spanning back up too30 days.</span></span>  <span data-ttu-id="091fe-206">Pracujeme tooexpand toto číslo tooinclude další události.</span><span class="sxs-lookup"><span data-stu-id="091fe-206">We are working tooexpand this number tooinclude more events.</span></span>
  >
  >
* <span data-ttu-id="091fe-207">**Otázka: jak daleko zpět sestav správy hesel hello přejděte?**</span><span class="sxs-lookup"><span data-stu-id="091fe-207">**Q:  How far back do hello password management reports go?**</span></span>

  > <span data-ttu-id="091fe-208">**Odpověď:** Správa hesel hello sestavy zobrazit operací, které se provádí v rámci hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="091fe-208">**A:** hello password management reports show operations occurring within hello last 30 days.</span></span> <span data-ttu-id="091fe-209">Teď Pokud potřebujete tooarchive tato data můžete stáhnout hello sestavy pravidelně a uložit je do samostatných umístění.</span><span class="sxs-lookup"><span data-stu-id="091fe-209">For now, if you need tooarchive this data, you can download hello reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="091fe-210">**Otázka: je maximální počet řádků, které se mohou objevit v sestavy správy hello heslo?**</span><span class="sxs-lookup"><span data-stu-id="091fe-210">**Q:  Is there a maximum number of rows that can appear on hello password management reports?**</span></span>

  > <span data-ttu-id="091fe-211">**Odpověď:** Ano, může se zobrazit nesmí být delší než 75 000 řádků na buď hello sestav správy hesel, jestli jsou uvedené v hello uživatelského rozhraní nebo stahování.</span><span class="sxs-lookup"><span data-stu-id="091fe-211">**A:** Yes, a maximum of 75,000 rows may appear on either of hello password management reports, whether they are being shown in hello UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="091fe-212">**Otázka: je k dispozici rozhraní API tooaccess hello heslo resetovat nebo registrační data pro generování sestav?**</span><span class="sxs-lookup"><span data-stu-id="091fe-212">**Q:  Is there an API tooaccess hello password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="091fe-213">**Odpověď:** Ano, najdete v části hello následující dokumentaci toolearn přístupu hello heslo resetovat vytváření sestav datového proudu.</span><span class="sxs-lookup"><span data-stu-id="091fe-213">**A:** Yes, see hello following documentation toolearn how you can access hello password reset reporting data stream.</span></span>  <span data-ttu-id="091fe-214">[Zjistěte, jak tooaccess resetování hesla události vytváření sestav prostřednictvím kódu programu](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="091fe-214">[Learn how tooaccess password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="091fe-215">Zpětný zápis hesla</span><span class="sxs-lookup"><span data-stu-id="091fe-215">Password writeback</span></span>
* <span data-ttu-id="091fe-216">**Otázka: jak zpětný zápis hesla funguje pozadí hello?**</span><span class="sxs-lookup"><span data-stu-id="091fe-216">**Q:  How does password writeback work behind hello scenes?**</span></span>

  > <span data-ttu-id="091fe-217">**Odpověď:** najdete v části [jak zpětný zápis hesla funguje](active-directory-passwords-writeback.md) pro vysvětlení, co se stane, když zapnete zpětný zápis hesla a jak se data proudí prostřednictvím systému hello zpět do místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="091fe-217">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through hello system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="091fe-218">**Otázka: jak dlouho trvá zpětný zápis hesla toowork?  Je k dispozici ke zpoždění synchronizace jako se synchronizací hodnoty hash hesla?**</span><span class="sxs-lookup"><span data-stu-id="091fe-218">**Q:  How long does password writeback take toowork?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="091fe-219">**Odpověď:** zpětný zápis hesla je rychlé.</span><span class="sxs-lookup"><span data-stu-id="091fe-219">**A:** Password writeback is instant.</span></span> <span data-ttu-id="091fe-220">Je synchronní kanál, který funguje zásadně jinak než synchronizaci hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="091fe-220">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="091fe-221">Zpětný zápis hesla umožňuje uživatelům tooget v reálném čase názor hello úspěch své heslo resetovat nebo změnit operaci.</span><span class="sxs-lookup"><span data-stu-id="091fe-221">Password writeback allows users tooget real-time feedback about hello success of their password reset or change operation.</span></span> <span data-ttu-id="091fe-222">Průměrná doba Hello pro úspěšné zpětný zápis hesla je v části 500 ms.</span><span class="sxs-lookup"><span data-stu-id="091fe-222">hello average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="091fe-223">**Otázka: Pokud Můj účet místní je, jak je můj účet nebo přístup do cloudu zneužitím?**</span><span class="sxs-lookup"><span data-stu-id="091fe-223">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="091fe-224">**Odpověď:** Pokud vaše místní ID je zakázaná, cloudu ID nebo přístup budou rovněž zakázány na další interval synchronizace hello prostřednictvím AAD Connect výchozí byt je to každých 30 minut.</span><span class="sxs-lookup"><span data-stu-id="091fe-224">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at hello next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="091fe-225">**Otázka: Pokud je omezené Můj účet místní zásady hesel místní služby Active Directory, SSPR orientují tuto zásadu po změně hesla hello?**</span><span class="sxs-lookup"><span data-stu-id="091fe-225">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change hello password?**</span></span>

  > <span data-ttu-id="091fe-226">**Odpověď:** Ano, spoléhá na SSPR a dodržuje hello místní zásady hesel služby AD, včetně typické domény zásady hesel služby AD, a také všechny definované zásady podrobné heslo cílové tooa zadaný uživatel.</span><span class="sxs-lookup"><span data-stu-id="091fe-226">**A:** Yes, SSPR relies on and abides by hello on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted tooa given user.</span></span>
  >
  >
* <span data-ttu-id="091fe-227">**Otázka: jaký typy účtů zpětný zápis hesla funguje pro?**</span><span class="sxs-lookup"><span data-stu-id="091fe-227">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="091fe-228">**Odpověď:** zpětný zápis hesla funguje pro federovaný a synchronizuje hodnoty Hash hesla uživatele.</span><span class="sxs-lookup"><span data-stu-id="091fe-228">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="091fe-229">**Otázka: zpětný zápis hesla vynutit zásady hesel Moje doména?**</span><span class="sxs-lookup"><span data-stu-id="091fe-229">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="091fe-230">**Odpověď:** Ano, zpětný zápis hesla vynucuje stáří hesla, historie, složitost, filtrů a další omezení, můžete mohou zavést na hesla v místní doméně.</span><span class="sxs-lookup"><span data-stu-id="091fe-230">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="091fe-231">**Otázka: je zpětný zápis hesla zabezpečení?  Jak se může být jistí, že nebude získat hacker I?**</span><span class="sxs-lookup"><span data-stu-id="091fe-231">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="091fe-232">**Odpověď:** Ano, jsou zabezpečené zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="091fe-232">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="091fe-233">tooread Další informace o hello čtyři vrstvy zabezpečení implementované hello služba zpětný zápis hesel, podívejte se na hello [model zabezpečení zpětný zápis hesla](active-directory-passwords-writeback.md#password-writeback-security-model) část v tom, jak zpětný zápis hesla funguje.</span><span class="sxs-lookup"><span data-stu-id="091fe-233">tooread more about hello four layers of security implemented by hello password writeback service, check out hello [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="091fe-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="091fe-234">Next steps</span></span>

<span data-ttu-id="091fe-235">Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="091fe-235">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="091fe-236">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="091fe-236">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="091fe-237">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="091fe-237">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="091fe-238">[**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel</span><span class="sxs-lookup"><span data-stu-id="091fe-238">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="091fe-239">[**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden</span><span class="sxs-lookup"><span data-stu-id="091fe-239">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="091fe-240">[**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="091fe-240">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="091fe-241">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="091fe-241">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="091fe-242">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="091fe-242">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="091fe-243">[**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.</span><span class="sxs-lookup"><span data-stu-id="091fe-243">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="091fe-244">[**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje</span><span class="sxs-lookup"><span data-stu-id="091fe-244">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="091fe-245">[**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR</span><span class="sxs-lookup"><span data-stu-id="091fe-245">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
