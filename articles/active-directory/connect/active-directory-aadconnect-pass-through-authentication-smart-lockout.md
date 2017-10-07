---
title: "Azure AD Connect: Ověřování průchozí – inteligentní uzamčení | Microsoft Docs"
description: "Tento článek popisuje, jak předávací ověřování Azure Active Directory (Azure AD) chrání před útoky hrubou silou hesla v cloudu hello místních účtů."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a><span data-ttu-id="56942-104">Předávací ověřování Azure Active Directory: Inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="56942-104">Azure Active Directory Pass-through Authentication: Smart Lockout</span></span>

## <a name="overview"></a><span data-ttu-id="56942-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="56942-105">Overview</span></span>

<span data-ttu-id="56942-106">Azure AD chrání před útoky hrubou silou heslo a originální uživatelům bránit v zamykají mimo jejich aplikace Office 365 a SaaS.</span><span class="sxs-lookup"><span data-stu-id="56942-106">Azure AD protects against brute force password attacks and prevents genuine users from being locked out of their Office 365 and SaaS applications.</span></span> <span data-ttu-id="56942-107">Tato funkce volána **inteligentní uzamčení**, je podporováno, pokud používáte ověřování průchozí jako způsob přihlášení.</span><span class="sxs-lookup"><span data-stu-id="56942-107">This capability, called **Smart Lockout**, is supported when you use Pass-through Authentication as your sign-in method.</span></span> <span data-ttu-id="56942-108">Inteligentní uzamčení je ve výchozím nastavení povolena pro všechny klienty a chráníte uživatelské účty všech hello čas; neexistuje žádné tooturn nutné ho na.</span><span class="sxs-lookup"><span data-stu-id="56942-108">Smart Lockout is enabled by default for all tenants and are protecting your user accounts all hello time; there is no need tooturn it on.</span></span>

<span data-ttu-id="56942-109">Inteligentní uzamčení funguje udržovat přehled o neúspěšných pokusů o přihlášení a po určitou **prahové hodnoty počtu uzamčení**, počáteční **doba trvání uzamčení**.</span><span class="sxs-lookup"><span data-stu-id="56942-109">Smart Lockout works by keeping track of failed sign-in attempts, and after a certain **Lockout Threshold**, starting a **Lockout Duration**.</span></span> <span data-ttu-id="56942-110">Všechny pokusů o přihlášení před útočníkem hello během hello doba trvání uzamčení odmítají.</span><span class="sxs-lookup"><span data-stu-id="56942-110">Any sign-in attempts from hello attacker during hello Lockout Duration are rejected.</span></span> <span data-ttu-id="56942-111">Pokud pokračuje hello útoku, hello následné neúspěšné pokusy o přihlášení po hello doba trvání uzamčení končí způsobí delší doby uzamčení.</span><span class="sxs-lookup"><span data-stu-id="56942-111">If hello attack continues, hello subsequent failed sign-in attempts after hello Lockout Duration ends result in longer Lockout Durations.</span></span>

>[!NOTE]
><span data-ttu-id="56942-112">Hello výchozí prahové hodnoty počtu uzamčení je 10 neúspěšných pokusů o přihlášení a hello výchozí doba trvání uzamčení je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="56942-112">hello default Lockout Threshold is 10 failed attempts, and hello default Lockout Duration is 60 seconds.</span></span>

<span data-ttu-id="56942-113">Inteligentní uzamčení také rozlišuje přihlášení z originálního uživatelů a útočníci a pouze zámky se útočníci hello ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="56942-113">Smart Lockout also distinguishes between sign-ins from genuine users and from attackers and only locks out hello attackers in most cases.</span></span> <span data-ttu-id="56942-114">Tato funkce zabrání útočníkům neoprávněnému uzamčení uživatele originálního.</span><span class="sxs-lookup"><span data-stu-id="56942-114">This functionality prevents attackers from maliciously locking out genuine users.</span></span> <span data-ttu-id="56942-115">Používáme po přihlášení chování uživatelů zařízení & prohlížečů a dalších toodistinguish signály mezi originální uživateli a útočníkům.</span><span class="sxs-lookup"><span data-stu-id="56942-115">We use past sign-in behavior, users’ devices & browsers and other signals toodistinguish between genuine users and attackers.</span></span> <span data-ttu-id="56942-116">Jsme jsou neustále zlepšování našich algoritmy.</span><span class="sxs-lookup"><span data-stu-id="56942-116">We are constantly improving our algorithms.</span></span>

<span data-ttu-id="56942-117">Protože předávací ověřování předává žádosti o ověření hesla do vaší místní služby Active Directory (AD), je třeba tooprevent útočníci z uzamykání účty AD vašich uživatelů.</span><span class="sxs-lookup"><span data-stu-id="56942-117">Because Pass-through Authentication forwards password validation requests onto your on-premises Active Directory (AD), you need tooprevent attackers from locking out your users’ AD accounts.</span></span> <span data-ttu-id="56942-118">Vzhledem k tomu, že máte vlastní zásady AD uzamčení účtu (konkrétně [ **prahovou hodnotu uzamknutí účtu** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) a [ **zásady resetování čítač účet uzamčení po** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), budete muset prahové hodnoty počtu uzamčení tooconfigure Azure AD a doba trvání uzamčení hodnoty správně toofilter out útoky v cloudu hello než dosáhnou místní AD.</span><span class="sxs-lookup"><span data-stu-id="56942-118">Since you have your own AD Account Lockout policies (specifically, [**Account Lockout Threshold**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) and [**Reset Account Lockout Counter After policies**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), you need tooconfigure Azure AD’s Lockout Threshold and Lockout Duration values appropriately toofilter out attacks in hello cloud before they reach your on-premises AD.</span></span>

>[!NOTE]
><span data-ttu-id="56942-119">Funkce uzamčení inteligentní Hello je zdarma a je _na_ ve výchozím nastavení pro všechny zákazníky.</span><span class="sxs-lookup"><span data-stu-id="56942-119">hello Smart Lockout feature is free and is _on_ by default for all customers.</span></span> <span data-ttu-id="56942-120">Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD však musí toohave alespoň jeden Azure AD Premium P2 licenci klienta.</span><span class="sxs-lookup"><span data-stu-id="56942-120">However, modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API needs your tenant toohave at least one Azure AD Premium P2 license.</span></span> <span data-ttu-id="56942-121">Nepotřebujete licenci Azure AD Premium P2 _na uživatele_ tooget hello inteligentní uzamčení funkce pomocí předávacího ověřování.</span><span class="sxs-lookup"><span data-stu-id="56942-121">You don't need an Azure AD Premium P2 license _per user_ tooget hello Smart Lockout feature with Pass-through Authentication.</span></span>

<span data-ttu-id="56942-122">tooensure, aby vaši uživatelé místní účty AD se dobře chráněný, musíte tooensure který:</span><span class="sxs-lookup"><span data-stu-id="56942-122">tooensure that your users’ on-premises AD accounts are well protected, you need tooensure that:</span></span>

1.  <span data-ttu-id="56942-123">Prahová hodnota pro uzamčení Azure AD je _méně_ než prahovou hodnotu uzamknutí účtu na AD.</span><span class="sxs-lookup"><span data-stu-id="56942-123">Azure AD’s Lockout Threshold is _less_ than AD’s Account Lockout Threshold.</span></span> <span data-ttu-id="56942-124">Doporučujeme nastavit hello hodnoty tak, aby na AD prahovou hodnotu uzamknutí účtu alespoň dvě nebo tři krát prahové hodnoty počtu uzamčení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56942-124">We recommend that you set hello values such that AD’s Account Lockout Threshold is at least two or three times Azure AD’s Lockout Threshold.</span></span>
2.  <span data-ttu-id="56942-125">Doba trvání uzamčení Azure AD (vyjádřený v sekundách) je _delší_ než AD na resetovat účet uzamčení čítač po (vyjádřený v minutách).</span><span class="sxs-lookup"><span data-stu-id="56942-125">Azure AD’s Lockout Duration (represented in seconds) is _longer_ than AD’s Reset Account Lockout Counter After (represented in minutes).</span></span>

## <a name="verify-your-ad-account-lockout-policies"></a><span data-ttu-id="56942-126">Zkontrolujte vaše zásady uzamčení účtu služby AD</span><span class="sxs-lookup"><span data-stu-id="56942-126">Verify your AD Account Lockout policies</span></span>

<span data-ttu-id="56942-127">Použijte následující pokyny tooverify hello zásad uzamčení účtu AD:</span><span class="sxs-lookup"><span data-stu-id="56942-127">Use hello following instructions tooverify your AD Account Lockout policies:</span></span>

1.  <span data-ttu-id="56942-128">Spusťte nástroj pro správu zásad skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="56942-128">Open hello Group Policy Management tool.</span></span>
2.  <span data-ttu-id="56942-129">Upravte hello zásad skupiny, které je použité tooall uživatele, například hello výchozí zásada domény.</span><span class="sxs-lookup"><span data-stu-id="56942-129">Edit hello group policy that is applied tooall users, for example, hello Default Domain Policy.</span></span>
3.  <span data-ttu-id="56942-130">Přejděte tooComputer Konfigurace uživatele\Zásady\Nastavení systému Windows\Nastavení zabezpečení\Zásady zamknutí uzamčení zásady.</span><span class="sxs-lookup"><span data-stu-id="56942-130">Navigate tooComputer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Account Lockout Policy.</span></span>
4.  <span data-ttu-id="56942-131">Ověřte hodnoty prahovou hodnotu uzamknutí účtu a resetování čítač účet uzamčení po.</span><span class="sxs-lookup"><span data-stu-id="56942-131">Verify your Account Lockout Threshold and Reset Account Lockout Counter After values.</span></span>

![Zásady uzamčení účtu AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a><span data-ttu-id="56942-133">Použít hello rozhraní Graph API toomanage hodnoty inteligentní uzamčení vašeho klienta</span><span class="sxs-lookup"><span data-stu-id="56942-133">Use hello Graph API toomanage your tenant’s Smart Lockout values</span></span>

>[!IMPORTANT]
><span data-ttu-id="56942-134">Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD je funkce Azure AD Premium P2.</span><span class="sxs-lookup"><span data-stu-id="56942-134">Modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API is an Azure AD Premium P2 feature.</span></span> <span data-ttu-id="56942-135">Je také potřebuje toobe jako globální správce v klientovi.</span><span class="sxs-lookup"><span data-stu-id="56942-135">It also needs you toobe a Global Administrator on your tenant.</span></span>

<span data-ttu-id="56942-136">Můžete použít [grafu Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, nastavte a aktualizujte hodnoty inteligentní uzamčení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56942-136">You can use [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, set, and update Azure AD’s Smart Lockout values.</span></span> <span data-ttu-id="56942-137">Ale můžete také provést tyto operace prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="56942-137">But you can also do these operations programmatically.</span></span>

### <a name="read-smart-lockout-values"></a><span data-ttu-id="56942-138">Inteligentní uzamčení pro čtení hodnoty</span><span class="sxs-lookup"><span data-stu-id="56942-138">Read Smart Lockout values</span></span>

<span data-ttu-id="56942-139">Postupujte podle těchto kroků tooread hodnoty inteligentní uzamčení vašeho klienta:</span><span class="sxs-lookup"><span data-stu-id="56942-139">Follow these steps tooread your tenant’s Smart Lockout values:</span></span>

1. <span data-ttu-id="56942-140">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="56942-140">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="56942-141">Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="56942-141">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="56942-142">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.</span><span class="sxs-lookup"><span data-stu-id="56942-142">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="56942-143">Žádost o rozhraní Graph API hello nakonfigurovat následujícím způsobem: nastavit verzi příliš "BETA" typ požadavku příliš "GET" a adresa URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="56942-143">Configure hello Graph API request as follows: Set version too“BETA”, request type too“GET” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="56942-144">Klikněte na tlačítko "Spustit dotaz" toosee hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="56942-144">Click "Run Query" toosee your tenant's Smart Lockout values.</span></span> <span data-ttu-id="56942-145">Pokud jste nenastavili před hodnoty vašeho klienta, zobrazí prázdnou sadou.</span><span class="sxs-lookup"><span data-stu-id="56942-145">If you haven't set your tenant's values before, you see an empty set.</span></span>

### <a name="set-smart-lockout-values"></a><span data-ttu-id="56942-146">Nastavení hodnot inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="56942-146">Set Smart Lockout values</span></span>

<span data-ttu-id="56942-147">Postupujte podle těchto kroků tooset hodnoty inteligentní uzamčení vašeho klienta (pro hello pouze poprvé):</span><span class="sxs-lookup"><span data-stu-id="56942-147">Follow these steps tooset your tenant’s Smart Lockout values (for hello first time only):</span></span>

1. <span data-ttu-id="56942-148">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="56942-148">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="56942-149">Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="56942-149">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="56942-150">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.</span><span class="sxs-lookup"><span data-stu-id="56942-150">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="56942-151">Žádost o rozhraní Graph API hello nakonfigurovat následujícím způsobem: nastavit verzi příliš "BETA" typ požadavku příliš "POST" a adresa URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="56942-151">Configure hello Graph API request as follows: Set version too“BETA”, request type too“POST” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="56942-152">Zkopírujte a vložte následující požadavku JSON do pole "Žádost text" hello hello.</span><span class="sxs-lookup"><span data-stu-id="56942-152">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="56942-153">Hello inteligentní uzamčení hodnoty podle potřeby změnit a použít náhodný identifikátor GUID pro `templateId`.</span><span class="sxs-lookup"><span data-stu-id="56942-153">Change hello Smart Lockout values as appropriate and use a random GUID for `templateId`.</span></span>
5. <span data-ttu-id="56942-154">Klikněte na tlačítko "Spustit dotaz" tooset hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="56942-154">Click "Run Query" tooset your tenant's Smart Lockout values.</span></span>

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
><span data-ttu-id="56942-155">Pokud je nepoužíváte, můžete nechat hello **BannedPasswordList** a **EnableBannedPasswordCheck** hodnoty jako prázdný ("") a "false" v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="56942-155">If you are not using them, you can leave hello **BannedPasswordList** and **EnableBannedPasswordCheck** values as empty ("") and "false" respectively.</span></span>

<span data-ttu-id="56942-156">Ověřte, že jste si nastavili hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="56942-156">Verify that you have set your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

### <a name="update-smart-lockout-values"></a><span data-ttu-id="56942-157">Aktualizujte hodnoty inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="56942-157">Update Smart Lockout values</span></span>

<span data-ttu-id="56942-158">Postupujte podle těchto kroků tooupdate hodnoty inteligentní uzamčení vašeho klienta (Pokud jste již zadali, je před):</span><span class="sxs-lookup"><span data-stu-id="56942-158">Follow these steps tooupdate your tenant’s Smart Lockout values (if you have already set them before):</span></span>

1. <span data-ttu-id="56942-159">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="56942-159">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="56942-160">Pokud se zobrazí výzva, udělení přístupu pro hello požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="56942-160">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="56942-161">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All" hello.</span><span class="sxs-lookup"><span data-stu-id="56942-161">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="56942-162">[Postupujte podle těchto kroků tooread vašeho klienta inteligentní uzamčení hodnoty](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="56942-162">[Follow these steps tooread your tenant's Smart Lockout values](#read-smart-lockout-values).</span></span> <span data-ttu-id="56942-163">Kopírování hello `id` hodnota (GUID) položky hello se zobrazovaným názvem"" jako "PasswordRuleSettings".</span><span class="sxs-lookup"><span data-stu-id="56942-163">Copy hello `id` value (a GUID) of hello item with "displayName" as "PasswordRuleSettings".</span></span>
4. <span data-ttu-id="56942-164">Hello rozhraní Graph API žádosti nakonfigurovat následujícím způsobem: nastavte verzi příliš "BETA" typ žádosti příliš "OPRAVIT" a adresy URL příliš`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -použít hello identifikátoru GUID z kroku 3 pro `<id>`.</span><span class="sxs-lookup"><span data-stu-id="56942-164">Configure hello Graph API request as follows: Set version too“BETA”, request type too“PATCH” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` - use hello GUID from Step 3 for `<id>`.</span></span>
5. <span data-ttu-id="56942-165">Zkopírujte a vložte následující požadavku JSON do pole "Žádost text" hello hello.</span><span class="sxs-lookup"><span data-stu-id="56942-165">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="56942-166">Hello inteligentní uzamčení hodnoty podle potřeby změnit.</span><span class="sxs-lookup"><span data-stu-id="56942-166">Change hello Smart Lockout values as appropriate.</span></span>
6. <span data-ttu-id="56942-167">Klikněte na tlačítko "Spustit dotaz" tooupdate hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="56942-167">Click "Run Query" tooupdate your tenant's Smart Lockout values.</span></span>

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

<span data-ttu-id="56942-168">Ověřte, že jste aktualizovali hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="56942-168">Verify that you have updated your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56942-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56942-169">Next steps</span></span>
- <span data-ttu-id="56942-170">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="56942-170">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
