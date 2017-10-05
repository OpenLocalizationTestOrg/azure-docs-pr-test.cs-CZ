---
title: "Změna klienta Azure Active Directory v Azure Remoteappu | Microsoft Docs"
description: "Změna klienta Azure Active Directory, které jsou přidružené k Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 20faf169-6e48-428a-8bdd-f231daff19fa
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 7c6c4ded8a11d8399968b2c32aff055d7f3ae5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a><span data-ttu-id="cd10a-103">Změna klienta Azure Active Directory v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="cd10a-103">Change the Azure Active Directory tenant in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cd10a-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="cd10a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="cd10a-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="cd10a-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="cd10a-106">Azure RemoteApp používá Azure Active Directory (Azure AD) pro povolení přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd10a-106">Azure RemoteApp uses Azure Active Directory (Azure AD) to allow user access.</span></span> <span data-ttu-id="cd10a-107">Klientovi pouze Azure AD, který můžete použít v Azure Remoteappu, která je spojená s předplatným služby Azure.</span><span class="sxs-lookup"><span data-stu-id="cd10a-107">The only Azure AD tenant that you can use in Azure RemoteApp is the one associated with the Azure subscription.</span></span> <span data-ttu-id="cd10a-108">Přidružené předplatné můžete zobrazit na **nastavení** na portálu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-108">You can view the associated subscription on the **Settings** page in the portal.</span></span> <span data-ttu-id="cd10a-109">Podívejte se na **Directory** sloupec **odběry** kartě.</span><span class="sxs-lookup"><span data-stu-id="cd10a-109">Look at the **Directory** column on the **Subscriptions** tab.</span></span>

> [!NOTE]
> <span data-ttu-id="cd10a-110">Než se změna neúspěšné, nejprve odebrat všechny uživatele z existujícího klienta Azure Active Directory ze všech kolekcí Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-110">For this change to succeed, first remove all users from the existing Azure Active Directory tenant from all Azure RemoteApp collections.</span></span> <span data-ttu-id="cd10a-111">Chcete-li to provést, přejděte na portál Azure, přejděte na **Azure RemoteApp** kartě a otevřete každou kolekci Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-111">To do this, go to the Azure Portal, go to the **Azure RemoteApp** tab and open every Azure RemoteApp collection.</span></span> <span data-ttu-id="cd10a-112">Přejděte na **uživatelé** kartě a odebrat uživatele, kteří patří ke klientovi aktuální Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cd10a-112">Go to the **Users** tab and remove users that belong to your current Azure Active Directory tenant.</span></span> <span data-ttu-id="cd10a-113">Opakujte pro všechny existující kolekce Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-113">Repeat for all existing Azure RemoteApp collections.</span></span> <span data-ttu-id="cd10a-114">Bez v takovém případě nebudete moci vytvořit ani opravovat kolekce.</span><span class="sxs-lookup"><span data-stu-id="cd10a-114">Without doing this, you will not be able to create or patch collections.</span></span>
> 
> 

<span data-ttu-id="cd10a-115">Pokud chcete použít jiný klienta, změna přidružení k předplatnému pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="cd10a-115">If you want to use a different tenant, use these steps to change the association with your subscription:</span></span>

1. <span data-ttu-id="cd10a-116">Na portálu odeberte všechny uživatele Azure AD, ke kterým jste dali přístup do kolekcí Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-116">In the portal, remove any Azure AD users to which you’ve given access to Azure RemoteApp collections.</span></span> <span data-ttu-id="cd10a-117">(Viz poznámka výše pokyny o tom, jak to udělat.)</span><span class="sxs-lookup"><span data-stu-id="cd10a-117">(See the note above for steps on how to do this.)</span></span>
2. <span data-ttu-id="cd10a-118">Nastavte účet Microsoft (dřív označovaných jako Live ID) jako správce služeb.</span><span class="sxs-lookup"><span data-stu-id="cd10a-118">Set a Microsoft account (formerly called a Live ID) as the Service administrator.</span></span> <span data-ttu-id="cd10a-119">(Nevíte, pokud již jste správcem služby?</span><span class="sxs-lookup"><span data-stu-id="cd10a-119">(Don't know if you already are the service admin?</span></span> <span data-ttu-id="cd10a-120">Můžete získat kliknutím **Nastavení -> správci**.) Nyní tady je způsob, změny:</span><span class="sxs-lookup"><span data-stu-id="cd10a-120">You can find out by clicking **Settings -> Administrators**.) Now, here's how you change that:</span></span>
   
   1. <span data-ttu-id="cd10a-121">Klikněte na uživatele v pravém horním rohu a pak klikněte na tlačítko **Zobrazit Moje faktury**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-121">Click the user in the upper right corner, and then click **View my bill**.</span></span>
   2. <span data-ttu-id="cd10a-122">Klikněte na předplatné.</span><span class="sxs-lookup"><span data-stu-id="cd10a-122">Click the subscription.</span></span> <span data-ttu-id="cd10a-123">Pak na nové stránce, posuňte se dolů a klikněte na tlačítko **upravit podrobnosti o předplatném** v pravém.</span><span class="sxs-lookup"><span data-stu-id="cd10a-123">Then, on the new page, scroll down and click **Edit subscription details** in the right.</span></span> <span data-ttu-id="cd10a-124">(Řazení vpravo, pokud tento střední dole vám pomůže ho najít.)</span><span class="sxs-lookup"><span data-stu-id="cd10a-124">(Sort of the middle bottom right, if that helps you find it.)</span></span>
   3. <span data-ttu-id="cd10a-125">Zadejte účet Microsoft pro uživatele, který by měl být správce služby.</span><span class="sxs-lookup"><span data-stu-id="cd10a-125">Type the Microsoft account for the user that should be the service admin.</span></span>
3. <span data-ttu-id="cd10a-126">Nyní se odhlásit z portálu a pak se přihlaste zpět pomocí účtu Microsoft, který jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="cd10a-126">Now, sign out of the portal, and then sign back in with the Microsoft account you specified in the previous step.</span></span>
4. <span data-ttu-id="cd10a-127">Klikněte na tlačítko **nový -> aplikace služby -> Active Directory -> Directory -> vlastní vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-127">Click **New -> App Services -> Active Directory -> Directory -> Custom Create**.</span></span>
5. <span data-ttu-id="cd10a-128">V části **Directory**, zvolte **použít existující adresář**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-128">Under **Directory**, choose **Use existing directory**.</span></span> <span data-ttu-id="cd10a-129">Vytvoříme máte nyní vás odhlásit z portálu, takže zvolte **nyní mě můžete odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-129">We're going to have to sign you out of the portal now, so choose **I am ready to be signed out now**.</span></span>
6. <span data-ttu-id="cd10a-130">Přihlaste se znovu do portálu jako globální správce adresáře, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="cd10a-130">Sign back into the portal as a global admin of the directory you want to add.</span></span> <span data-ttu-id="cd10a-131">(Pokud již nebyly jste globální správce, bude po zaokrouhlit z Přihlaste se a přihlaste.)</span><span class="sxs-lookup"><span data-stu-id="cd10a-131">(If you weren't already a global admin, you will be after a round of sign in and then sign out.)</span></span>
7. <span data-ttu-id="cd10a-132">Zobrazí se dotaz, při přihlášení Pokud chcete použít existující klientovi AD s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="cd10a-132">You'll be asked when you sign in if you want to use your existing AD tenant with your subscription.</span></span> <span data-ttu-id="cd10a-133">Klikněte na tlačítko **pokračovat**a potom klikněte na **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-133">Click **Continue**, and then click **Sign out now**.</span></span>
8. <span data-ttu-id="cd10a-134">Přihlaste se zpět znovu a vraťte se zpátky a **Nastavení -> odběry**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-134">Sign back in again, and go back to **Settings -> Subscriptions**.</span></span> <span data-ttu-id="cd10a-135">Vyberte předplatné a pak klikněte na **upravit adresář**.</span><span class="sxs-lookup"><span data-stu-id="cd10a-135">Select your subscription, and then click **Edit Directory**.</span></span> <span data-ttu-id="cd10a-136">Vyberte klienta Azure AD, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="cd10a-136">Select the Azure AD tenant that you want to use.</span></span>

<span data-ttu-id="cd10a-137">Můžete teď používání nové Azure AD klienta pro řízení přístupu k předplatnému Azure a ke konfiguraci přístupu uživatele v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="cd10a-137">You can now use the new Azure AD tenant to control access to the Azure subscription and to configure user access in Azure RemoteApp.</span></span>

