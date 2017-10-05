---
title: "Jak přenést vaše jednotlivé licencované uživatele do skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak přepínat z jednotlivých uživatelských licencí na základě skupiny licencí pomocí služby Azure Active Directory pro"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b77dd4e9a6d361a05382397e89b575896fdad84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a><span data-ttu-id="3806a-104">Postup přidání licencovaní uživatelé pro skupinu pro licencování v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3806a-104">How to add licensed users to a group for licensing in Azure Active Directory</span></span>

<span data-ttu-id="3806a-105">Máte stávající licence, nasazení uživatelům v organizacích prostřednictvím "přímé přiřazení"; To znamená přiřadit licence pro jednotlivé uživatele pomocí skriptů prostředí PowerShell nebo jiných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="3806a-105">You may have existing licenses deployed to users in the organizations via “direct assignment”; that is, using PowerShell scripts or other tools to assign individual user licenses.</span></span> <span data-ttu-id="3806a-106">Pokud chcete začít používat na základě skupiny licencí ke správě licencí ve vaší organizaci, budete potřebovat plán migrace bezproblémově nahradit existující řešení na základě skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="3806a-106">If you would like to start using group-based licensing to manage licenses in your organization, you will need a migration plan to seamlessly replace existing solutions with group-based licensing.</span></span>

<span data-ttu-id="3806a-107">Nejdůležitější třeba vzít v úvahu je, že vyhněte situaci, kdy migrace na základě skupiny licencí pro nebudou uživatelé dočasně došlo ke ztrátě jejich aktuálně přiřazené licence.</span><span class="sxs-lookup"><span data-stu-id="3806a-107">The most important thing to keep in mind is that you should avoid a situation where migrating to group-based licensing will result in users temporarily losing their currently assigned licenses.</span></span> <span data-ttu-id="3806a-108">Chcete-li odebrat riziko ztráty přístupu ke službám a jejich data uživatelů je nutno jakýkoli proces, který může mít za následek odebrání licencí.</span><span class="sxs-lookup"><span data-stu-id="3806a-108">Any process that may result in removal of licenses should be avoided to remove the risk of users losing access to services and their data.</span></span>

## <a name="recommended-migration-process"></a><span data-ttu-id="3806a-109">Proces migrace doporučené</span><span class="sxs-lookup"><span data-stu-id="3806a-109">Recommended migration process</span></span>

1. <span data-ttu-id="3806a-110">Máte existující automatizace (například prostředí PowerShell) Správa licencí přiřazení a odebrání pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3806a-110">You have existing automation (for example, PowerShell) managing license assignment and removal for users.</span></span> <span data-ttu-id="3806a-111">Necháte spuštěné, jako je.</span><span class="sxs-lookup"><span data-stu-id="3806a-111">Leave it running as is.</span></span>

2. <span data-ttu-id="3806a-112">Vytvořit novou skupinu licencování (nebo rozhodnout, které stávající skupiny pro použití) a ujistěte se, že všechny požadované uživatelé jsou přidány jako členové.</span><span class="sxs-lookup"><span data-stu-id="3806a-112">Create a new licensing group (or decide which existing groups to use) and make sure that all required users are added as members.</span></span>

3. <span data-ttu-id="3806a-113">Přiřadit požadované licence na tyto skupiny. vaším cílem mělo být tak, aby odrážela stejné licencování stavu, ve kterém na uživatele, je použití vaší existující automatizace (například prostředí PowerShell).</span><span class="sxs-lookup"><span data-stu-id="3806a-113">Assign the required licenses to those groups; your goal should be to reflect the same licensing state your existing automation (for example, PowerShell) is applying to those users.</span></span>

4. <span data-ttu-id="3806a-114">Ověřte, že licence byla použita pro všechny uživatele v těchto skupinách.</span><span class="sxs-lookup"><span data-stu-id="3806a-114">Verify that licenses have been applied to all users in those groups.</span></span> <span data-ttu-id="3806a-115">To lze provést kontrolu stavu zpracování pro každou skupinu a kontroly protokolů auditu.</span><span class="sxs-lookup"><span data-stu-id="3806a-115">This can be done by checking the processing state on each group and by checking Audit Logs.</span></span>

  - <span data-ttu-id="3806a-116">Můžete místo kontrola jednotlivých uživatelů podle jejich licenčních podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3806a-116">You can spot check individual users by looking at their license details.</span></span> <span data-ttu-id="3806a-117">Zobrazí se, že mají stejný "přímo" přiřazené licence a "zděděná" ze skupin.</span><span class="sxs-lookup"><span data-stu-id="3806a-117">You will see that they have the same licenses assigned “directly” and “inherited” from groups.</span></span>

  - <span data-ttu-id="3806a-118">Můžete spustit skript prostředí PowerShell k [ověřte, jak jsou licence přiřazovat k uživatelům](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span><span class="sxs-lookup"><span data-stu-id="3806a-118">You can run a PowerShell script to [verify how licenses are assigned to users](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span></span>

  - <span data-ttu-id="3806a-119">Když stejné licenci produktu je přiřazen k uživateli obě, přímo a prostřednictvím skupiny, je pouze jednu licenci spotřebovávána uživatele.</span><span class="sxs-lookup"><span data-stu-id="3806a-119">When the same product license is assigned to the user both directly and through a group, only one license is consumed by the user.</span></span> <span data-ttu-id="3806a-120">Proto žádné další licence, které jsou potřebná k provedení migrace.</span><span class="sxs-lookup"><span data-stu-id="3806a-120">Hence no additional licenses are required to perform migration.</span></span>

5. <span data-ttu-id="3806a-121">Ověřte, že žádná přiřazení licence se nezdařilo kontrolou každou skupinu pro uživatele v chybovém stavu.</span><span class="sxs-lookup"><span data-stu-id="3806a-121">Verify that no license assignments failed by checking each group for users in error state.</span></span> <span data-ttu-id="3806a-122">Další informace najdete v tématu [identifikuje a řešení potíží s licencí pro skupinu](active-directory-licensing-group-problem-resolution-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3806a-122">For more information, see [Identifying and resolving license problems for a group](active-directory-licensing-group-problem-resolution-azure-portal.md).</span></span>

6. <span data-ttu-id="3806a-123">Zvažte odebrání původní přímé přiřazení; Můžete to udělat postupně, "témata", nejprve monitorování výsledek na určitou podskupinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3806a-123">Consider removing the original direct assignments; you may want to do it gradually, in “waves”, to monitor the outcome on a subset of users first.</span></span>

  <span data-ttu-id="3806a-124">Původní přímé přiřazení může nechat na uživatele, ale když uživatelé nechte jejich licencovanou skupiny stále zachovají původní licenci, která může být, aby, že chcete.</span><span class="sxs-lookup"><span data-stu-id="3806a-124">You could leave the original direct assignments on users, but when the users leave their licensed groups they will still retain the original license, which is possibly not want you want.</span></span>

## <a name="an-example"></a><span data-ttu-id="3806a-125">Příklad</span><span class="sxs-lookup"><span data-stu-id="3806a-125">An example</span></span>

<span data-ttu-id="3806a-126">Máme organizaci s 1 000 uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3806a-126">We have an organization with 1,000 users.</span></span> <span data-ttu-id="3806a-127">Všichni uživatelé vyžadují Enterprise Mobility + Security (EMS) licence.</span><span class="sxs-lookup"><span data-stu-id="3806a-127">All users require Enterprise Mobility + Security (EMS) licenses.</span></span> <span data-ttu-id="3806a-128">200 uživatele z finančního oddělení a vyžadují Office 365 Enterprise E3 licence.</span><span class="sxs-lookup"><span data-stu-id="3806a-128">200 users are in the Finance Department and require Office 365 Enterprise E3 licenses.</span></span> <span data-ttu-id="3806a-129">Máme skript prostředí PowerShell, který je spuštěn místně přidávání a odebírání licence od uživatelů, dokud budou pocházet a přejděte.</span><span class="sxs-lookup"><span data-stu-id="3806a-129">We have a PowerShell script running on premises adding and removing licenses from users as they come and go.</span></span> <span data-ttu-id="3806a-130">Chceme nahraďte skript na základě skupiny licencí, licence jsou spravovány automaticky službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3806a-130">We want to replace the script with group-based licensing so licenses are managed automatically by Azure AD.</span></span>

<span data-ttu-id="3806a-131">Zde je, jak může vypadat proces migrace:</span><span class="sxs-lookup"><span data-stu-id="3806a-131">Here is what the migration process could look like:</span></span>

1. <span data-ttu-id="3806a-132">Pomocí portálu Azure přiřadit licence EMS pro **všichni uživatelé** skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3806a-132">Using the Azure portal assign the EMS license to the **All users** group in Azure AD.</span></span> <span data-ttu-id="3806a-133">Přiřadit licence E3 na **finančního oddělení** skupinu, která obsahuje všechny požadované uživatele.</span><span class="sxs-lookup"><span data-stu-id="3806a-133">Assign the E3 license to the **Finance department** group that contains all the required users.</span></span>

2. <span data-ttu-id="3806a-134">Pro každou skupinu potvrďte, že se dokončila přiřazení licence pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="3806a-134">For each group, confirm that license assignment has completed for all users.</span></span> <span data-ttu-id="3806a-135">Přejděte do okna pro každou skupinu, vyberte **licence**a zkontrolujte stav zpracování v horní části **licence** okno.</span><span class="sxs-lookup"><span data-stu-id="3806a-135">Go to the blade for each group, select **Licenses**, and check the processing status at the top of the **Licenses** blade.</span></span>

  - <span data-ttu-id="3806a-136">Vyhledejte "Nejnovější licence změny provedeny pro všechny uživatele" potvrďte zpracování byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="3806a-136">Look for “Latest license changes have been applied to all users" to confirm processing has completed.</span></span>

  - <span data-ttu-id="3806a-137">Hledejte v horní části o všechny uživatele, pro které licence pravděpodobně nebyla přiřazena úspěšně oznámení.</span><span class="sxs-lookup"><span data-stu-id="3806a-137">Look for a notification on top about any users for whom licenses may have not been successfully assigned.</span></span> <span data-ttu-id="3806a-138">Jsme dostatek licencí pro některé uživatele?</span><span class="sxs-lookup"><span data-stu-id="3806a-138">Did we run out of licenses for some users?</span></span> <span data-ttu-id="3806a-139">Mají někteří uživatelé konfliktní licence SKU, které brání dědění skupiny licencí?</span><span class="sxs-lookup"><span data-stu-id="3806a-139">Do some users have conflicting license SKUs that prevent them from inheriting group licenses?</span></span>

3. <span data-ttu-id="3806a-140">Místo zkontrolujte někteří uživatelé ověřit, zda mají obě direct a skupinu licencí použít.</span><span class="sxs-lookup"><span data-stu-id="3806a-140">Spot check some users to verify that they have both the direct and group licenses applied.</span></span> <span data-ttu-id="3806a-141">Přejděte do okna pro uživatele, vyberte **licence**a zkontrolujte stav licence.</span><span class="sxs-lookup"><span data-stu-id="3806a-141">Go to the blade for a user, select **Licenses**, and examine the state of licenses.</span></span>

  - <span data-ttu-id="3806a-142">Toto je očekávané uživatelské stavu během migrace:</span><span class="sxs-lookup"><span data-stu-id="3806a-142">This is the expected user state during migration:</span></span>

      ![Stav očekávané uživatele](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  <span data-ttu-id="3806a-144">Tím potvrdí, že uživatel má přímé a zděděné licence.</span><span class="sxs-lookup"><span data-stu-id="3806a-144">This confirms that the user has both direct and inherited licenses.</span></span> <span data-ttu-id="3806a-145">Vidíte, jak **EMS** a **E3** jsou přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="3806a-145">We see that both **EMS** and **E3** are assigned.</span></span>

  - <span data-ttu-id="3806a-146">Vyberte každé licence a zobrazit podrobnosti o povolené služby.</span><span class="sxs-lookup"><span data-stu-id="3806a-146">Select each license to show details about the enabled services.</span></span> <span data-ttu-id="3806a-147">To slouží ke kontrole, pokud direct a skupinu licencí povolit přesně stejnou službu plány pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3806a-147">This can be used to check if the direct and group licenses enable exactly the same service plans for the user.</span></span>

      ![Zkontrolujte plány služeb](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. <span data-ttu-id="3806a-149">Po potvrzení, že odpovídají direct a skupinu licencí, můžete začít odebrání přímé licence od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3806a-149">After confirming that both direct and group licenses are equivalent, you can start removing direct licenses from users.</span></span> <span data-ttu-id="3806a-150">Můžete si otestovat to jejich odebráním pro jednotlivé uživatele v portálu a spusťte skripty pro automatizaci tak, aby měl je v hromadné odebrána.</span><span class="sxs-lookup"><span data-stu-id="3806a-150">You can test this by removing them for individual users in the portal and then run automation scripts to have them removed in bulk.</span></span> <span data-ttu-id="3806a-151">Tady je příklad stejného uživatele s přímé licence odebrat prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="3806a-151">Here is an example of the same user with the direct licenses removed through the portal.</span></span> <span data-ttu-id="3806a-152">Všimněte si, že stav licence zůstává beze změny, ale už vidíme přímé přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3806a-152">Notice that the license state remains unchanged, but we no longer see direct assignments.</span></span>

  ![odebrat přímý licencí](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a><span data-ttu-id="3806a-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3806a-154">Next steps</span></span>

<span data-ttu-id="3806a-155">Další informace o scénáře pro správu licencí pomocí skupin, přečtěte si téma</span><span class="sxs-lookup"><span data-stu-id="3806a-155">To learn more about other scenarios for license management through groups, read</span></span>

* [<span data-ttu-id="3806a-156">Přiřazování licencí pro skupinu v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3806a-156">Assigning licenses to a group in Azure Active Directory</span></span>](active-directory-licensing-group-assignment-azure-portal.md)
* [<span data-ttu-id="3806a-157">Co je na základě skupin licencování v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3806a-157">What is group-based licensing in Azure Active Directory?</span></span>](active-directory-licensing-whatis-azure-portal.md)
* [<span data-ttu-id="3806a-158">Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3806a-158">Identifying and resolving license problems for a group in Azure Active Directory</span></span>](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [<span data-ttu-id="3806a-159">Azure Active Directory na základě skupin licencí další scénáře</span><span class="sxs-lookup"><span data-stu-id="3806a-159">Azure Active Directory group-based licensing additional scenarios</span></span>](active-directory-licensing-group-advanced.md)
