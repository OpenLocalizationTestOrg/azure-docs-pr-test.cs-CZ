---
title: "Azure AD Connect: Ověřování průchozí – inteligentní uzamčení | Microsoft Docs"
description: "Tento článek popisuje, jak předávací ověřování Azure Active Directory (Azure AD) chrání vaše místní účty před útoky hrubou silou hesla v cloudu."
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
ms.openlocfilehash: c84b2406e6373701c83c509342129bd6d7d4034b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a><span data-ttu-id="a9580-104">Předávací ověřování Azure Active Directory: Inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="a9580-104">Azure Active Directory Pass-through Authentication: Smart Lockout</span></span>

## <a name="overview"></a><span data-ttu-id="a9580-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a9580-105">Overview</span></span>

<span data-ttu-id="a9580-106">Azure AD chrání před útoky hrubou silou heslo a originální uživatelům bránit v zamykají mimo jejich aplikace Office 365 a SaaS.</span><span class="sxs-lookup"><span data-stu-id="a9580-106">Azure AD protects against brute force password attacks and prevents genuine users from being locked out of their Office 365 and SaaS applications.</span></span> <span data-ttu-id="a9580-107">Tato funkce volána **inteligentní uzamčení**, je podporováno, pokud používáte ověřování průchozí jako způsob přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a9580-107">This capability, called **Smart Lockout**, is supported when you use Pass-through Authentication as your sign-in method.</span></span> <span data-ttu-id="a9580-108">Inteligentní uzamčení je ve výchozím nastavení povolena pro všechny klienty a chráníte uživatelské účty všech čas; není potřeba ji zapnout.</span><span class="sxs-lookup"><span data-stu-id="a9580-108">Smart Lockout is enabled by default for all tenants and are protecting your user accounts all the time; there is no need to turn it on.</span></span>

<span data-ttu-id="a9580-109">Inteligentní uzamčení funguje udržovat přehled o neúspěšných pokusů o přihlášení a po určitou **prahové hodnoty počtu uzamčení**, počáteční **doba trvání uzamčení**.</span><span class="sxs-lookup"><span data-stu-id="a9580-109">Smart Lockout works by keeping track of failed sign-in attempts, and after a certain **Lockout Threshold**, starting a **Lockout Duration**.</span></span> <span data-ttu-id="a9580-110">Všechny pokusů o přihlášení před útočníkem během doby trvání uzamčení odmítají.</span><span class="sxs-lookup"><span data-stu-id="a9580-110">Any sign-in attempts from the attacker during the Lockout Duration are rejected.</span></span> <span data-ttu-id="a9580-111">Pokud pokračuje útoku, následné neúspěšné pokusy o přihlášení po dobu trvání uzamčení končí způsobí delší doby uzamčení.</span><span class="sxs-lookup"><span data-stu-id="a9580-111">If the attack continues, the subsequent failed sign-in attempts after the Lockout Duration ends result in longer Lockout Durations.</span></span>

>[!NOTE]
><span data-ttu-id="a9580-112">Prahové hodnoty počtu uzamčení výchozí hodnota je 10 neúspěšných pokusů o přihlášení, a doba trvání uzamčení výchozí hodnota je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9580-112">The default Lockout Threshold is 10 failed attempts, and the default Lockout Duration is 60 seconds.</span></span>

<span data-ttu-id="a9580-113">Inteligentní uzamčení také rozlišuje přihlášení z originálního uživatelů a útočníci a pouze zámky se útočníci ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="a9580-113">Smart Lockout also distinguishes between sign-ins from genuine users and from attackers and only locks out the attackers in most cases.</span></span> <span data-ttu-id="a9580-114">Tato funkce zabrání útočníkům neoprávněnému uzamčení uživatele originálního.</span><span class="sxs-lookup"><span data-stu-id="a9580-114">This functionality prevents attackers from maliciously locking out genuine users.</span></span> <span data-ttu-id="a9580-115">Používáme po přihlášení chování, zařízení uživatelů & prohlížeče a jinými signály odlišit uživatele originálního a útočníkům.</span><span class="sxs-lookup"><span data-stu-id="a9580-115">We use past sign-in behavior, users’ devices & browsers and other signals to distinguish between genuine users and attackers.</span></span> <span data-ttu-id="a9580-116">Jsme jsou neustále zlepšování našich algoritmy.</span><span class="sxs-lookup"><span data-stu-id="a9580-116">We are constantly improving our algorithms.</span></span>

<span data-ttu-id="a9580-117">Vzhledem k tomu, že předávací ověřování předává žádosti o ověření hesla do vaší místní služby Active Directory (AD), budete muset útočníkům zabránit v uzamykání účty AD vašich uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a9580-117">Because Pass-through Authentication forwards password validation requests onto your on-premises Active Directory (AD), you need to prevent attackers from locking out your users’ AD accounts.</span></span> <span data-ttu-id="a9580-118">Vzhledem k tomu, že máte vlastní zásady AD uzamčení účtu (konkrétně [ **prahovou hodnotu uzamknutí účtu** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) a [ **zásady resetování čítač účet uzamčení po** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), budete muset nakonfigurovat prahové hodnoty počtu uzamčení Azure AD a doba trvání uzamčení hodnoty správně filtrovat útoky v cloudu než dosáhnou místní AD.</span><span class="sxs-lookup"><span data-stu-id="a9580-118">Since you have your own AD Account Lockout policies (specifically, [**Account Lockout Threshold**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) and [**Reset Account Lockout Counter After policies**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), you need to configure Azure AD’s Lockout Threshold and Lockout Duration values appropriately to filter out attacks in the cloud before they reach your on-premises AD.</span></span>

>[!NOTE]
><span data-ttu-id="a9580-119">Funkce uzamčení inteligentní je zdarma a je _na_ ve výchozím nastavení pro všechny zákazníky.</span><span class="sxs-lookup"><span data-stu-id="a9580-119">The Smart Lockout feature is free and is _on_ by default for all customers.</span></span> <span data-ttu-id="a9580-120">Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD však musí mít alespoň jednu licenci Azure AD Premium P2 vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="a9580-120">However, modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API needs your tenant to have at least one Azure AD Premium P2 license.</span></span> <span data-ttu-id="a9580-121">Nepotřebujete licenci Azure AD Premium P2 _na uživatele_ získat funkci inteligentního uzamčení pomocí předávacího ověřování.</span><span class="sxs-lookup"><span data-stu-id="a9580-121">You don't need an Azure AD Premium P2 license _per user_ to get the Smart Lockout feature with Pass-through Authentication.</span></span>

<span data-ttu-id="a9580-122">Zajistit, aby vaši uživatelé místní účty AD se dobře chráněný, musíte zajistit, aby:</span><span class="sxs-lookup"><span data-stu-id="a9580-122">To ensure that your users’ on-premises AD accounts are well protected, you need to ensure that:</span></span>

1.  <span data-ttu-id="a9580-123">Prahová hodnota pro uzamčení Azure AD je _méně_ než prahovou hodnotu uzamknutí účtu na AD.</span><span class="sxs-lookup"><span data-stu-id="a9580-123">Azure AD’s Lockout Threshold is _less_ than AD’s Account Lockout Threshold.</span></span> <span data-ttu-id="a9580-124">Doporučujeme nastavit hodnoty tak, aby na AD prahovou hodnotu uzamknutí účtu alespoň dvě nebo tři krát prahové hodnoty počtu uzamčení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9580-124">We recommend that you set the values such that AD’s Account Lockout Threshold is at least two or three times Azure AD’s Lockout Threshold.</span></span>
2.  <span data-ttu-id="a9580-125">Doba trvání uzamčení Azure AD (vyjádřený v sekundách) je _delší_ než AD na resetovat účet uzamčení čítač po (vyjádřený v minutách).</span><span class="sxs-lookup"><span data-stu-id="a9580-125">Azure AD’s Lockout Duration (represented in seconds) is _longer_ than AD’s Reset Account Lockout Counter After (represented in minutes).</span></span>

## <a name="verify-your-ad-account-lockout-policies"></a><span data-ttu-id="a9580-126">Zkontrolujte vaše zásady uzamčení účtu služby AD</span><span class="sxs-lookup"><span data-stu-id="a9580-126">Verify your AD Account Lockout policies</span></span>

<span data-ttu-id="a9580-127">Postupujte podle následujících pokynů k ověření zásad uzamčení účtu AD:</span><span class="sxs-lookup"><span data-stu-id="a9580-127">Use the following instructions to verify your AD Account Lockout policies:</span></span>

1.  <span data-ttu-id="a9580-128">Spusťte nástroj pro správu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="a9580-128">Open the Group Policy Management tool.</span></span>
2.  <span data-ttu-id="a9580-129">Upravte zásady skupiny, které se použije pro všechny uživatele, například výchozí zásada domény.</span><span class="sxs-lookup"><span data-stu-id="a9580-129">Edit the group policy that is applied to all users, for example, the Default Domain Policy.</span></span>
3.  <span data-ttu-id="a9580-130">Přejděte na Konfigurace uživatele\Zásady\Nastavení systému Windows\Nastavení zabezpečení\Zásady zamknutí zásada pro uzamčení počítače.</span><span class="sxs-lookup"><span data-stu-id="a9580-130">Navigate to Computer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Account Lockout Policy.</span></span>
4.  <span data-ttu-id="a9580-131">Ověřte hodnoty prahovou hodnotu uzamknutí účtu a resetování čítač účet uzamčení po.</span><span class="sxs-lookup"><span data-stu-id="a9580-131">Verify your Account Lockout Threshold and Reset Account Lockout Counter After values.</span></span>

![Zásady uzamčení účtu AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values"></a><span data-ttu-id="a9580-133">Použití rozhraní Graph API ke správě vašeho klienta inteligentní uzamčení hodnoty</span><span class="sxs-lookup"><span data-stu-id="a9580-133">Use the Graph API to manage your tenant’s Smart Lockout values</span></span>

>[!IMPORTANT]
><span data-ttu-id="a9580-134">Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD je funkce Azure AD Premium P2.</span><span class="sxs-lookup"><span data-stu-id="a9580-134">Modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API is an Azure AD Premium P2 feature.</span></span> <span data-ttu-id="a9580-135">Je také musí být globálním správcem na vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="a9580-135">It also needs you to be a Global Administrator on your tenant.</span></span>

<span data-ttu-id="a9580-136">Můžete použít [grafu Explorer](https://developer.microsoft.com/graph/graph-explorer) číst, nastavit a aktualizujte hodnoty inteligentní uzamčení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9580-136">You can use [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) to read, set, and update Azure AD’s Smart Lockout values.</span></span> <span data-ttu-id="a9580-137">Ale můžete také provést tyto operace prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a9580-137">But you can also do these operations programmatically.</span></span>

### <a name="read-smart-lockout-values"></a><span data-ttu-id="a9580-138">Inteligentní uzamčení pro čtení hodnoty</span><span class="sxs-lookup"><span data-stu-id="a9580-138">Read Smart Lockout values</span></span>

<span data-ttu-id="a9580-139">Postupujte podle těchto kroků načíst hodnoty inteligentní uzamčení vašeho klienta:</span><span class="sxs-lookup"><span data-stu-id="a9580-139">Follow these steps to read your tenant’s Smart Lockout values:</span></span>

1. <span data-ttu-id="a9580-140">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="a9580-140">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="a9580-141">Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a9580-141">If prompted, grant access for the requested permissions.</span></span>
2. <span data-ttu-id="a9580-142">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".</span><span class="sxs-lookup"><span data-stu-id="a9580-142">Click “Modify permissions” and select the “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="a9580-143">Žádost o rozhraní Graph API nakonfigurovat následujícím způsobem: nastavit verzi "Beta verze", typ požadavku na "GET" a adresu URL pro `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="a9580-143">Configure the Graph API request as follows: Set version to “BETA”, request type to “GET” and URL to `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="a9580-144">Klikněte na tlačítko "Spustit dotaz" zobrazíte hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="a9580-144">Click "Run Query" to see your tenant's Smart Lockout values.</span></span> <span data-ttu-id="a9580-145">Pokud jste nenastavili před hodnoty vašeho klienta, zobrazí prázdnou sadou.</span><span class="sxs-lookup"><span data-stu-id="a9580-145">If you haven't set your tenant's values before, you see an empty set.</span></span>

### <a name="set-smart-lockout-values"></a><span data-ttu-id="a9580-146">Nastavení hodnot inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="a9580-146">Set Smart Lockout values</span></span>

<span data-ttu-id="a9580-147">Postupujte podle těchto kroků můžete nastavte hodnoty inteligentní uzamčení vašeho klienta (poprvé pouze):</span><span class="sxs-lookup"><span data-stu-id="a9580-147">Follow these steps to set your tenant’s Smart Lockout values (for the first time only):</span></span>

1. <span data-ttu-id="a9580-148">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="a9580-148">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="a9580-149">Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a9580-149">If prompted, grant access for the requested permissions.</span></span>
2. <span data-ttu-id="a9580-150">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".</span><span class="sxs-lookup"><span data-stu-id="a9580-150">Click “Modify permissions” and select the “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="a9580-151">Nakonfigurujte rozhraní Graph API požadavek takto: nastavte verzi na "BETA", "POST" a adresa URL pro typ žádosti `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="a9580-151">Configure the Graph API request as follows: Set version to “BETA”, request type to “POST” and URL to `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="a9580-152">Zkopírujte a vložte následující požadavku JSON do pole "Textu žádosti".</span><span class="sxs-lookup"><span data-stu-id="a9580-152">Copy and paste the following JSON request into the "Request Body" field.</span></span> <span data-ttu-id="a9580-153">Inteligentní uzamčení hodnoty podle potřeby změnit a použít náhodný identifikátor GUID pro `templateId`.</span><span class="sxs-lookup"><span data-stu-id="a9580-153">Change the Smart Lockout values as appropriate and use a random GUID for `templateId`.</span></span>
5. <span data-ttu-id="a9580-154">Klikněte na tlačítko "Spustit dotaz" nastavit hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="a9580-154">Click "Run Query" to set your tenant's Smart Lockout values.</span></span>

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
><span data-ttu-id="a9580-155">Pokud je nepoužíváte, můžete nechat **BannedPasswordList** a **EnableBannedPasswordCheck** hodnoty jako prázdný ("") a "false" v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a9580-155">If you are not using them, you can leave the **BannedPasswordList** and **EnableBannedPasswordCheck** values as empty ("") and "false" respectively.</span></span>

<span data-ttu-id="a9580-156">Ověřte, že jste si nastavili hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="a9580-156">Verify that you have set your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

### <a name="update-smart-lockout-values"></a><span data-ttu-id="a9580-157">Aktualizujte hodnoty inteligentní uzamčení</span><span class="sxs-lookup"><span data-stu-id="a9580-157">Update Smart Lockout values</span></span>

<span data-ttu-id="a9580-158">Postupujte podle těchto kroků provedete aktualizaci hodnoty inteligentní uzamčení vašeho klienta (Pokud jste již zadali, je před):</span><span class="sxs-lookup"><span data-stu-id="a9580-158">Follow these steps to update your tenant’s Smart Lockout values (if you have already set them before):</span></span>

1. <span data-ttu-id="a9580-159">Průzkumník grafu se přihlaste jako globální správce tenanta.</span><span class="sxs-lookup"><span data-stu-id="a9580-159">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="a9580-160">Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a9580-160">If prompted, grant access for the requested permissions.</span></span>
2. <span data-ttu-id="a9580-161">Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".</span><span class="sxs-lookup"><span data-stu-id="a9580-161">Click “Modify permissions” and select the “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="a9580-162">[Postupujte podle těchto kroků načíst hodnoty inteligentní uzamčení vašeho klienta](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="a9580-162">[Follow these steps to read your tenant's Smart Lockout values](#read-smart-lockout-values).</span></span> <span data-ttu-id="a9580-163">Kopírování `id` hodnota (GUID) položky se zobrazovaným názvem"" jako "PasswordRuleSettings".</span><span class="sxs-lookup"><span data-stu-id="a9580-163">Copy the `id` value (a GUID) of the item with "displayName" as "PasswordRuleSettings".</span></span>
4. <span data-ttu-id="a9580-164">Rozhraní Graph API žádosti nakonfigurovat následujícím způsobem: nastavit verzi "Beta verze", typ požadavku na "OPRAVIT" a adresu URL pro `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -pomocí identifikátoru GUID z kroku 3 pro `<id>`.</span><span class="sxs-lookup"><span data-stu-id="a9580-164">Configure the Graph API request as follows: Set version to “BETA”, request type to “PATCH” and URL to `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` - use the GUID from Step 3 for `<id>`.</span></span>
5. <span data-ttu-id="a9580-165">Zkopírujte a vložte následující požadavku JSON do pole "Textu žádosti".</span><span class="sxs-lookup"><span data-stu-id="a9580-165">Copy and paste the following JSON request into the "Request Body" field.</span></span> <span data-ttu-id="a9580-166">Inteligentní uzamčení hodnoty podle potřeby změňte.</span><span class="sxs-lookup"><span data-stu-id="a9580-166">Change the Smart Lockout values as appropriate.</span></span>
6. <span data-ttu-id="a9580-167">Klikněte na tlačítko "Spustit dotaz" aktualizujte hodnoty inteligentní uzamčení vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="a9580-167">Click "Run Query" to update your tenant's Smart Lockout values.</span></span>

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

<span data-ttu-id="a9580-168">Ověřte, že jste aktualizovali hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="a9580-168">Verify that you have updated your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9580-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9580-169">Next steps</span></span>
- <span data-ttu-id="a9580-170">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="a9580-170">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
