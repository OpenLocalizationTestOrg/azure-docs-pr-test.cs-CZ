---
title: "Azure Active Directory, vytváření sestav – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy týkající se vytváření sestav Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: accf292f70bf0eafdefc00c3feeaf8e346605401
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="fda26-103">Nejčastější dotazy týkající se vytváření sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fda26-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="fda26-104">Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) o vytváření sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fda26-104">This article includes answers to frequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="fda26-105">Další podrobnosti najdete v tématu [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fda26-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="fda26-106">**Otázka: co je uchovávání dat protokoly aktivity (auditu a přihlášení) na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fda26-106">**Q: What is the data retention for activity logs (Audit and Sign-ins) in the Azure portal?**</span></span> 

<span data-ttu-id="fda26-107">**Odpověď:** poskytujeme 7 dnů dat pro naše zákazníky volné a přepnutím do Azure AD Premium 1 nebo Premium 2 licenci, můžete přístup k datům po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="fda26-107">**A:** We provide 7 days of data for our free customers and by switching to an Azure AD Premium 1 or Premium 2 license, you can access data for up to 30 days.</span></span> <span data-ttu-id="fda26-108">Další informace o uchovávání dat v [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="fda26-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="fda26-109">**Otázka: jak dlouho trvá až po dokončení Moje úloh data aktivit uvidí?**</span><span class="sxs-lookup"><span data-stu-id="fda26-109">**Q: How long does it take until I can see the Activity data after I have completed my task?**</span></span>

<span data-ttu-id="fda26-110">**Odpověď:** protokoly auditu aktivity, jejichž latence od 15 minut až hodinu.</span><span class="sxs-lookup"><span data-stu-id="fda26-110">**A:** Audit activity logs have a latency ranging from 15 mins to an hour.</span></span> <span data-ttu-id="fda26-111">Protokoly přihlašovací aktivity, jejichž latence od 15 minut pro většinu záznamů a až 2 hodiny pro několik záznamů.</span><span class="sxs-lookup"><span data-stu-id="fda26-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up to 2 hours for a few records.</span></span>

---

<span data-ttu-id="fda26-112">**Otázka: nutné být globálním správcem zobrazíte aktivity protokoly na portálu Azure a získat data prostřednictvím rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="fda26-112">**Q: Do I need to be a global admin to see the activity logs in the Azure Portal or to get data through the API?**</span></span>

<span data-ttu-id="fda26-113">**Odpověď:** Ne.</span><span class="sxs-lookup"><span data-stu-id="fda26-113">**A:** No.</span></span> <span data-ttu-id="fda26-114">Může být buď **zabezpečení čtečky**, **správce zabezpečení** nebo **globálního správce** chcete zobrazit data na portálu Azure nebo k ní přistupují přes rozhraní API pro vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="fda26-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** to see reporting data in Azure Portal or by accessing it through the API.</span></span>

---

<span data-ttu-id="fda26-115">**Otázka: je možné získat informace o protokolu činnosti Office 365 prostřednictvím portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fda26-115">**Q: Can I get Office 365 activity log information through the Azure portal?**</span></span>

<span data-ttu-id="fda26-116">**Odpověď:** i když aktivita Office 365 a Azure AD aktivity protokoly sdílení velké množství prostředků adresáře, pokud chcete, aby ucelený pohled na protokoly aktivity Office 365, by měl přejdete do do centra pro správu Office 365 se získat informace o protokolu Office 365 aktivity.</span><span class="sxs-lookup"><span data-stu-id="fda26-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of the directory resources, if you want a full view of the Office 365 activity logs, you should go to the Office 365 Admin Center to get Office 365 Activity log information.</span></span>

---


<span data-ttu-id="fda26-117">**Otázka: které rozhraní API se používá k načtení informací o protokoly aktivity Office 365?**</span><span class="sxs-lookup"><span data-stu-id="fda26-117">**Q: Which APIs do I use to get information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="fda26-118">**Odpověď:** používat pro přístup k rozhraní API pro správu Office 365 [Office 365 aktivity protokoly prostřednictvím rozhraní API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="fda26-118">**A:** Use the Office 365 Management APIs to access the [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="fda26-119">**Otázka: počet záznamů, můžete stáhnout z portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fda26-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="fda26-120">**Odpověď:** až 120 kB záznamy si můžete stáhnout z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fda26-120">**A:** You can download up to 120K records from the Azure portal.</span></span> <span data-ttu-id="fda26-121">Záznamy jsou seřazené podle *nejnovější* a ve výchozím nastavení, můžete získat nejnovější záznamy 120 kB.</span><span class="sxs-lookup"><span data-stu-id="fda26-121">The records are sorted by *most recent* and by default, you get the most recent 120K records.</span></span> 

---

<span data-ttu-id="fda26-122">**Otázka: počet záznamů, můžete dotazovat pomocí aktivity rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="fda26-122">**Q: How many records can I query through the activities API?**</span></span>

<span data-ttu-id="fda26-123">**Odpověď:** můžete dát dotaz na záznamy až 1 milion (Pokud nechcete použít operátor top, která seřadí záznam většina poslední).</span><span class="sxs-lookup"><span data-stu-id="fda26-123">**A:** You can query up to 1 million records (if you don’t use the top operator, which sorts the record by most recent).</span></span> <span data-ttu-id="fda26-124">Pokud používáte operátor "top", se můžete dotazovat až 500 kB záznamy.</span><span class="sxs-lookup"><span data-stu-id="fda26-124">If you do use the “top” operator, you can query up to 500K records.</span></span> <span data-ttu-id="fda26-125">Můžete najít ukázkové dotazy týkající se používání rozhraní API zde [zde](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fda26-125">You can find sample queries on how to use the API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="fda26-126">**Otázka: jak lze získat licenci premium?**</span><span class="sxs-lookup"><span data-stu-id="fda26-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="fda26-127">**Odpověď:** najdete v části [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md) pro odpověď na tuto otázku.</span><span class="sxs-lookup"><span data-stu-id="fda26-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer to this question.</span></span>

---

<span data-ttu-id="fda26-128">**Otázka: jak brzy by měl zobrazit data aktivity po získání licence premium?**</span><span class="sxs-lookup"><span data-stu-id="fda26-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="fda26-129">**Odpověď:** Pokud již máte data aktivity, jako volné licenci, pak můžete zobrazit stejná data.</span><span class="sxs-lookup"><span data-stu-id="fda26-129">**A:** If you already have activities data as a free license, then you can see the same data.</span></span> <span data-ttu-id="fda26-130">Pokud nemáte k dispozici žádná data, pak bude trvat jeden nebo dva dny.</span><span class="sxs-lookup"><span data-stu-id="fda26-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="fda26-131">**Otázka: Po získání licenci Azure AD premium zobrazit data poslední měsíc?**</span><span class="sxs-lookup"><span data-stu-id="fda26-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="fda26-132">**Odpověď:** Pokud Přepnuli jste nedávno na verzi Premium (včetně zkušební verzi), zobrazí se data až do 7 dnů původně.</span><span class="sxs-lookup"><span data-stu-id="fda26-132">**A:** If you have recently switched to a Premium version (including a trial version), you can see data up to 7 days initially.</span></span> <span data-ttu-id="fda26-133">Když se data hromadí, zobrazí se až 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="fda26-133">When data accumulates, you will see up to 30 days.</span></span>

---

<span data-ttu-id="fda26-134">**Otázka: je riziko událostí v ochrany identit, ale nejsou zobrazeny odpovídající přihlášení v všechny přihlášení. Je to očekávané?**</span><span class="sxs-lookup"><span data-stu-id="fda26-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in the all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="fda26-135">**Odpověď:** Ano, Identity Protection vyhodnotí riziko pro všechny toky ověřování jestli-li být interaktivní nebo neinteraktivní.</span><span class="sxs-lookup"><span data-stu-id="fda26-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="fda26-136">Ale všechny přihlášení pouze sestava zobrazí jenom interaktivní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="fda26-136">However, all sign-ins only report shows only the interactive sign-ins.</span></span>

---

<span data-ttu-id="fda26-137">**Otázka: jak můžete stáhnout "Uživatelé označení příznakem rizik" sestav na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fda26-137">**Q: How can I download the “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="fda26-138">**Odpověď:** možnost stažení *uživatelé označení příznakem rizik* sestavy přidá brzy.</span><span class="sxs-lookup"><span data-stu-id="fda26-138">**A:** The option to download *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="fda26-139">**Otázka: Jak poznám, proč přihlášení, nebo pro uživatele byla příznakem rizikové na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fda26-139">**Q: How do I know why a sign-in or a user was flagged risky in the Azure portal?**</span></span>

<span data-ttu-id="fda26-140">**Odpověď:** Premium edition zákazníkům další informace o základní rizikových událostech kliknutím na uživatele v "Uživatelé označení příznakem rizik" nebo kliknutím na "rizikové přihlášení".</span><span class="sxs-lookup"><span data-stu-id="fda26-140">**A:** Premium edition customers can learn more about the underlying risk events by clicking on the user in “Users flagged for risk” or by clicking on the “Risky sign-ins”.</span></span> <span data-ttu-id="fda26-141">Zákazníci volné a Základní edice získat zobrazíte rizikové uživatele a přihlášení bez základní informace o události riziko.</span><span class="sxs-lookup"><span data-stu-id="fda26-141">Free and Basic edition customers get to see the at-risk users and sign-ins without the underlying risk event information.</span></span>

---

<span data-ttu-id="fda26-142">**Otázka: jak jsou vypočítávány IP adresy v sestavě rizikové přihlášení a přihlášení??**</span><span class="sxs-lookup"><span data-stu-id="fda26-142">**Q: How are IP addresses calculated in the sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="fda26-143">**Odpověď:** IP adresy vydávají tak, že není spolehlivý připojení mezi IP adresu a kde se fyzicky nacházejí počítači s touto adresou.</span><span class="sxs-lookup"><span data-stu-id="fda26-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where the computer with that address is physically located.</span></span> <span data-ttu-id="fda26-144">To ztěžuje faktorech například mobilní poskytovatelů a vydání z Centrální fondy IP adres velmi často daleko od skutečně použití klientské zařízení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="fda26-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where the client device is actually used.</span></span> <span data-ttu-id="fda26-145">Z výše uvedených převodu IP adresu na fyzické umístění je nejlepší úsilí na základě trasování, data registru, zpětné vyhledání a další informace.</span><span class="sxs-lookup"><span data-stu-id="fda26-145">Given the above, converting IP address to a physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
