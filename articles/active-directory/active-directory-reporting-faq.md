---
title: "aaaAzure Active Directory Reporting – nejčastější dotazy | Microsoft Docs"
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
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="fd30e-103">Nejčastější dotazy týkající se vytváření sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fd30e-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="fd30e-104">Tento článek obsahuje odpovědi toofrequently kladené dotazy (FAQ) o vytváření sestav Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fd30e-104">This article includes answers toofrequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="fd30e-105">Další podrobnosti najdete v tématu [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fd30e-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="fd30e-106">**Otázka: co je uchovávání dat hello protokoly aktivity (auditu a přihlášení) v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-106">**Q: What is hello data retention for activity logs (Audit and Sign-ins) in hello Azure portal?**</span></span> 

<span data-ttu-id="fd30e-107">**Odpověď:** poskytujeme 7 dnů dat pro naše zákazníky volné a přepnutím tooan Azure AD Premium 1 nebo Premium 2 licenci, přistupujete k datům pro až too30 dnů.</span><span class="sxs-lookup"><span data-stu-id="fd30e-107">**A:** We provide 7 days of data for our free customers and by switching tooan Azure AD Premium 1 or Premium 2 license, you can access data for up too30 days.</span></span> <span data-ttu-id="fd30e-108">Další informace o uchovávání dat v [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="fd30e-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="fd30e-109">**Otázka: jak dlouho trvá až po dokončení Moje úloh data aktivit hello uvidí?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-109">**Q: How long does it take until I can see hello Activity data after I have completed my task?**</span></span>

<span data-ttu-id="fd30e-110">**Odpověď:** protokoly auditu aktivity, jejichž latence od hodinu tooan 15 minut.</span><span class="sxs-lookup"><span data-stu-id="fd30e-110">**A:** Audit activity logs have a latency ranging from 15 mins tooan hour.</span></span> <span data-ttu-id="fd30e-111">Protokoly přihlašovací aktivity, jejichž latence od 15 minut pro většinu záznamů a v provozu too2 hodin pro několik záznamů.</span><span class="sxs-lookup"><span data-stu-id="fd30e-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up too2 hours for a few records.</span></span>

---

<span data-ttu-id="fd30e-112">**Otázka: je nutné toobe protokoly aktivitu globálního správce toosee hello hello portálu Azure nebo tooget data prostřednictvím hello rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-112">**Q: Do I need toobe a global admin toosee hello activity logs in hello Azure Portal or tooget data through hello API?**</span></span>

<span data-ttu-id="fd30e-113">**Odpověď:** Ne.</span><span class="sxs-lookup"><span data-stu-id="fd30e-113">**A:** No.</span></span> <span data-ttu-id="fd30e-114">Může být buď **zabezpečení čtečky**, **správce zabezpečení** nebo **globálního správce** toosee data na portálu Azure nebo k ní přistupují prostřednictvím hello rozhraní API pro vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="fd30e-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** toosee reporting data in Azure Portal or by accessing it through hello API.</span></span>

---

<span data-ttu-id="fd30e-115">**Otázka: je možné získat informace o protokolu činnosti Office 365 prostřednictvím hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-115">**Q: Can I get Office 365 activity log information through hello Azure portal?**</span></span>

<span data-ttu-id="fd30e-116">**Odpověď:** i když aktivita Office 365 a Azure AD aktivity protokoly sdílení velké množství prostředků directory hello, pokud chcete, aby ucelený pohled na dobrý den protokoly aktivity Office 365, měli byste toohello Centrum pro správu Office 365 tooget Office 365 aktivity protokolu informace.</span><span class="sxs-lookup"><span data-stu-id="fd30e-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of hello directory resources, if you want a full view of hello Office 365 activity logs, you should go toohello Office 365 Admin Center tooget Office 365 Activity log information.</span></span>

---


<span data-ttu-id="fd30e-117">**Otázka: rozhraní API se který používám tooget informace o protokoly aktivity Office 365?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-117">**Q: Which APIs do I use tooget information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="fd30e-118">**Odpověď:** použití hello rozhraní API pro správu Office 365 tooaccess hello [Office 365 aktivity protokoly prostřednictvím rozhraní API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="fd30e-118">**A:** Use hello Office 365 Management APIs tooaccess hello [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="fd30e-119">**Otázka: počet záznamů, můžete stáhnout z portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="fd30e-120">**Odpověď:** si můžete stáhnout si too120K záznamy ze hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fd30e-120">**A:** You can download up too120K records from hello Azure portal.</span></span> <span data-ttu-id="fd30e-121">Hello záznamy jsou seřazené podle *nejnovější* a ve výchozím nastavení, získáte záznamy hello nejnovější 120 kB.</span><span class="sxs-lookup"><span data-stu-id="fd30e-121">hello records are sorted by *most recent* and by default, you get hello most recent 120K records.</span></span> 

---

<span data-ttu-id="fd30e-122">**Otázka: počet záznamů, můžete dotazovat pomocí aktivity hello rozhraní API?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-122">**Q: How many records can I query through hello activities API?**</span></span>

<span data-ttu-id="fd30e-123">**Odpověď:** až too1 mil. záznamy se můžete dotazovat (Pokud nechcete použít operátor top hello, která seřadí hello záznam většina poslední).</span><span class="sxs-lookup"><span data-stu-id="fd30e-123">**A:** You can query up too1 million records (if you don’t use hello top operator, which sorts hello record by most recent).</span></span> <span data-ttu-id="fd30e-124">Pokud se rozhodnete použít operátor "top" hello, se můžete dotazovat až too500K záznamy.</span><span class="sxs-lookup"><span data-stu-id="fd30e-124">If you do use hello “top” operator, you can query up too500K records.</span></span> <span data-ttu-id="fd30e-125">Ukázkové dotazy na tom, jak toouse hello API Zde můžete najít [zde](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fd30e-125">You can find sample queries on how toouse hello API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="fd30e-126">**Otázka: jak lze získat licenci premium?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="fd30e-127">**Odpověď:** najdete v části [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md) pro otázku toothis odpovědí.</span><span class="sxs-lookup"><span data-stu-id="fd30e-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer toothis question.</span></span>

---

<span data-ttu-id="fd30e-128">**Otázka: jak brzy by měl zobrazit data aktivity po získání licence premium?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="fd30e-129">**Odpověď:** Pokud již máte data aktivity, jako volné licenci, pak se zobrazí hello stejná data.</span><span class="sxs-lookup"><span data-stu-id="fd30e-129">**A:** If you already have activities data as a free license, then you can see hello same data.</span></span> <span data-ttu-id="fd30e-130">Pokud nemáte k dispozici žádná data, pak bude trvat jeden nebo dva dny.</span><span class="sxs-lookup"><span data-stu-id="fd30e-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="fd30e-131">**Otázka: Po získání licenci Azure AD premium zobrazit data poslední měsíc?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="fd30e-132">**Odpověď:** Pokud Přepnuli jste nedávno tooa verze Premium (včetně zkušební verzi), můžete zobrazit data si too7 dnů původně.</span><span class="sxs-lookup"><span data-stu-id="fd30e-132">**A:** If you have recently switched tooa Premium version (including a trial version), you can see data up too7 days initially.</span></span> <span data-ttu-id="fd30e-133">Když se data hromadí, zobrazí se až too30 dnů.</span><span class="sxs-lookup"><span data-stu-id="fd30e-133">When data accumulates, you will see up too30 days.</span></span>

---

<span data-ttu-id="fd30e-134">**Otázka: je riziko událostí v ochrany identit, ale odpovídající přihlášení v hello nejsou zobrazeny všechny přihlášení. Je to očekávané?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in hello all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="fd30e-135">**Odpověď:** Ano, Identity Protection vyhodnotí riziko pro všechny toky ověřování jestli-li být interaktivní nebo neinteraktivní.</span><span class="sxs-lookup"><span data-stu-id="fd30e-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="fd30e-136">Ale všechny přihlášení pouze sestava zobrazí jenom hello interaktivní přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fd30e-136">However, all sign-ins only report shows only hello interactive sign-ins.</span></span>

---

<span data-ttu-id="fd30e-137">**Otázka: jak můžete stáhnout hello "Uživatelé označení příznakem rizik" sestav na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-137">**Q: How can I download hello “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="fd30e-138">**Odpověď:** hello možnost toodownload *uživatelé označení příznakem rizik* sestavy přidá brzy.</span><span class="sxs-lookup"><span data-stu-id="fd30e-138">**A:** hello option toodownload *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="fd30e-139">**Otázka: Jak poznám, proč přihlášení, nebo pro uživatele byla příznakem rizikové v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="fd30e-139">**Q: How do I know why a sign-in or a user was flagged risky in hello Azure portal?**</span></span>

<span data-ttu-id="fd30e-140">**Odpověď:** zákazníkům edice Premium můžete další informace o hello základní rizikových událostech kliknutím na uživatele hello v "Uživatelé označení příznakem rizik" nebo kliknutím na hello "Rizikové přihlášení".</span><span class="sxs-lookup"><span data-stu-id="fd30e-140">**A:** Premium edition customers can learn more about hello underlying risk events by clicking on hello user in “Users flagged for risk” or by clicking on hello “Risky sign-ins”.</span></span> <span data-ttu-id="fd30e-141">Zákazníci volné a Základní edice získání bez hello základní informace o události riziko toosee hello rizikové uživatelů a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="fd30e-141">Free and Basic edition customers get toosee hello at-risk users and sign-ins without hello underlying risk event information.</span></span>

---

<span data-ttu-id="fd30e-142">**Otázka: jak jsou vypočítávány IP adresy v sestavě rizikové přihlášení a přihlášení hello??**</span><span class="sxs-lookup"><span data-stu-id="fd30e-142">**Q: How are IP addresses calculated in hello sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="fd30e-143">**Odpověď:** IP adresy vydávají tak, že není spolehlivý připojení mezi IP adresu a kde hello s touto adresou se fyzicky nacházejí.</span><span class="sxs-lookup"><span data-stu-id="fd30e-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where hello computer with that address is physically located.</span></span> <span data-ttu-id="fd30e-144">To ztěžuje faktorech například mobilní poskytovatelů a vydání z Centrální fondy IP adres velmi často daleko od skutečně použití hello klientské zařízení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="fd30e-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where hello client device is actually used.</span></span> <span data-ttu-id="fd30e-145">Zadané hello výše, převod IP adres tooa fyzické umístění je nejlepší úsilí na základě trasování, data registru, zpětné vyhledání a další informace.</span><span class="sxs-lookup"><span data-stu-id="fd30e-145">Given hello above, converting IP address tooa physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
