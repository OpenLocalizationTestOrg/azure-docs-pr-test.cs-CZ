---
title: "aaaHow toomigrate vaše jednotlivé licencovaní uživatelé tooa skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak tooswitch z jednotlivých uživatelských licencí, na základě toogroup licencování pomocí služby Azure Active Directory"
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
ms.openlocfilehash: 25e5c760b7e632ba71edde10d937fe580aa6ed35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-licensed-users-tooa-group-for-licensing-in-azure-active-directory"></a><span data-ttu-id="d9035-104">Jak tooadd licencuje tooa skupiny uživatelů pro licencování v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9035-104">How tooadd licensed users tooa group for licensing in Azure Active Directory</span></span>

<span data-ttu-id="d9035-105">Existující toousers licence, nasazení může mít v organizacích hello prostřednictvím "přímé přiřazení"; To znamená pomocí skriptů prostředí PowerShell nebo jiných nástrojů tooassign jednotlivých uživatelských licencí.</span><span class="sxs-lookup"><span data-stu-id="d9035-105">You may have existing licenses deployed toousers in hello organizations via “direct assignment”; that is, using PowerShell scripts or other tools tooassign individual user licenses.</span></span> <span data-ttu-id="d9035-106">Pokud chcete toostart pomocí na základě skupiny licencování toomanage licencí ve vaší organizaci, budete potřebovat migrace plánu tooseamlessly nahradit existující řešení na základě skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="d9035-106">If you would like toostart using group-based licensing toomanage licenses in your organization, you will need a migration plan tooseamlessly replace existing solutions with group-based licensing.</span></span>

<span data-ttu-id="d9035-107">Hello nejdůležitějších věcí tookeep v paměti je, že vyhněte situaci, kdy licencování migrace na základě toogroup nebudou uživatelé dočasně došlo ke ztrátě jejich aktuálně přiřazené licence.</span><span class="sxs-lookup"><span data-stu-id="d9035-107">hello most important thing tookeep in mind is that you should avoid a situation where migrating toogroup-based licensing will result in users temporarily losing their currently assigned licenses.</span></span> <span data-ttu-id="d9035-108">Jakýkoli proces, který může mít za následek odebrání licencí by měla být vyhnout tooremove hello riziko ztráty přístupu tooservices a jejich data uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9035-108">Any process that may result in removal of licenses should be avoided tooremove hello risk of users losing access tooservices and their data.</span></span>

## <a name="recommended-migration-process"></a><span data-ttu-id="d9035-109">Proces migrace doporučené</span><span class="sxs-lookup"><span data-stu-id="d9035-109">Recommended migration process</span></span>

1. <span data-ttu-id="d9035-110">Máte existující automatizace (například prostředí PowerShell) Správa licencí přiřazení a odebrání pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9035-110">You have existing automation (for example, PowerShell) managing license assignment and removal for users.</span></span> <span data-ttu-id="d9035-111">Necháte spuštěné, jako je.</span><span class="sxs-lookup"><span data-stu-id="d9035-111">Leave it running as is.</span></span>

2. <span data-ttu-id="d9035-112">Vytvořit novou skupinu licencování (nebo rozhodnout, které stávající skupiny toouse) a ujistěte se, že všechny požadované uživatelé jsou přidány jako členové.</span><span class="sxs-lookup"><span data-stu-id="d9035-112">Create a new licensing group (or decide which existing groups toouse) and make sure that all required users are added as members.</span></span>

3. <span data-ttu-id="d9035-113">Přiřazení licencí hello požadované skupiny toothose; vaším cílem mělo být tooreflect hello stejné licencování stavu vaší existující automatizace (například prostředí PowerShell) je použití toothose uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9035-113">Assign hello required licenses toothose groups; your goal should be tooreflect hello same licensing state your existing automation (for example, PowerShell) is applying toothose users.</span></span>

4. <span data-ttu-id="d9035-114">Ověřte, že licence byla použité tooall uživatelé v těchto skupinách.</span><span class="sxs-lookup"><span data-stu-id="d9035-114">Verify that licenses have been applied tooall users in those groups.</span></span> <span data-ttu-id="d9035-115">To lze provést kontrolou stavu hello zpracování pro každou skupinu a kontrolou protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="d9035-115">This can be done by checking hello processing state on each group and by checking Audit Logs.</span></span>

  - <span data-ttu-id="d9035-116">Můžete místo kontrola jednotlivých uživatelů podle jejich licenčních podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d9035-116">You can spot check individual users by looking at their license details.</span></span> <span data-ttu-id="d9035-117">Uvidíte, že mají hello stejné "přímo" přiřazené licence a "zděděná" ze skupin.</span><span class="sxs-lookup"><span data-stu-id="d9035-117">You will see that they have hello same licenses assigned “directly” and “inherited” from groups.</span></span>

  - <span data-ttu-id="d9035-118">Můžete spustit skript prostředí PowerShell příliš[ověřte přidělování toousers licence](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span><span class="sxs-lookup"><span data-stu-id="d9035-118">You can run a PowerShell script too[verify how licenses are assigned toousers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span></span>

  - <span data-ttu-id="d9035-119">Když hello stejné produktu licence se nepřiřadí uživatele toohello přímo a současně prostřednictvím skupiny, je pouze jednu licenci spotřebovávána hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9035-119">When hello same product license is assigned toohello user both directly and through a group, only one license is consumed by hello user.</span></span> <span data-ttu-id="d9035-120">Proto nejsou žádné další licence požadované tooperform migrace.</span><span class="sxs-lookup"><span data-stu-id="d9035-120">Hence no additional licenses are required tooperform migration.</span></span>

5. <span data-ttu-id="d9035-121">Ověřte, že žádná přiřazení licence se nezdařilo kontrolou každou skupinu pro uživatele v chybovém stavu.</span><span class="sxs-lookup"><span data-stu-id="d9035-121">Verify that no license assignments failed by checking each group for users in error state.</span></span> <span data-ttu-id="d9035-122">Další informace najdete v tématu [identifikuje a řešení potíží s licencí pro skupinu](active-directory-licensing-group-problem-resolution-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9035-122">For more information, see [Identifying and resolving license problems for a group](active-directory-licensing-group-problem-resolution-azure-portal.md).</span></span>

6. <span data-ttu-id="d9035-123">Zvažte odebrání původní přímé přiřazení hello; může být vhodné toodo postupně, v "vlnami", toomonitor text hello výsledek na určitou podskupinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9035-123">Consider removing hello original direct assignments; you may want toodo it gradually, in “waves”, toomonitor hello outcome on a subset of users first.</span></span>

  <span data-ttu-id="d9035-124">Může necháte hello původní přímé přiřazení na uživatele, ale když někteří uživatelé hello opustí jejich skupin licencí, které bude stále zachovávají původní licenci hello, která může být, aby, že chcete.</span><span class="sxs-lookup"><span data-stu-id="d9035-124">You could leave hello original direct assignments on users, but when hello users leave their licensed groups they will still retain hello original license, which is possibly not want you want.</span></span>

## <a name="an-example"></a><span data-ttu-id="d9035-125">Příklad</span><span class="sxs-lookup"><span data-stu-id="d9035-125">An example</span></span>

<span data-ttu-id="d9035-126">Máme organizaci s 1 000 uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9035-126">We have an organization with 1,000 users.</span></span> <span data-ttu-id="d9035-127">Všichni uživatelé vyžadují Enterprise Mobility + Security (EMS) licence.</span><span class="sxs-lookup"><span data-stu-id="d9035-127">All users require Enterprise Mobility + Security (EMS) licenses.</span></span> <span data-ttu-id="d9035-128">200 uživatelů jsou v hello finančního oddělení a vyžadují Office 365 Enterprise E3 licence.</span><span class="sxs-lookup"><span data-stu-id="d9035-128">200 users are in hello Finance Department and require Office 365 Enterprise E3 licenses.</span></span> <span data-ttu-id="d9035-129">Máme skript prostředí PowerShell, který je spuštěn místně přidávání a odebírání licence od uživatelů, dokud budou pocházet a přejděte.</span><span class="sxs-lookup"><span data-stu-id="d9035-129">We have a PowerShell script running on premises adding and removing licenses from users as they come and go.</span></span> <span data-ttu-id="d9035-130">Chceme tooreplace hello skript na základě skupiny licencí, licence jsou spravovány automaticky službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9035-130">We want tooreplace hello script with group-based licensing so licenses are managed automatically by Azure AD.</span></span>

<span data-ttu-id="d9035-131">Tady je proces migrace jaké hello může vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="d9035-131">Here is what hello migration process could look like:</span></span>

1. <span data-ttu-id="d9035-132">Pomocí portálu Azure přiřadit hello EMS licence toohello hello **všichni uživatelé** skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9035-132">Using hello Azure portal assign hello EMS license toohello **All users** group in Azure AD.</span></span> <span data-ttu-id="d9035-133">Přiřazení licencí toohello hello E3 **finančního oddělení** skupinu, která obsahuje všechny uživatele, vyžaduje hello.</span><span class="sxs-lookup"><span data-stu-id="d9035-133">Assign hello E3 license toohello **Finance department** group that contains all hello required users.</span></span>

2. <span data-ttu-id="d9035-134">Pro každou skupinu potvrďte, že se dokončila přiřazení licence pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9035-134">For each group, confirm that license assignment has completed for all users.</span></span> <span data-ttu-id="d9035-135">Okno přejděte toohello pro každou skupinu, vyberte **licence**a zkontrolujte stav zpracování hello hello horní části hello **licence** okno.</span><span class="sxs-lookup"><span data-stu-id="d9035-135">Go toohello blade for each group, select **Licenses**, and check hello processing status at hello top of hello **Licenses** blade.</span></span>

  - <span data-ttu-id="d9035-136">Podívejte se pro "nejnovější změny licencí byly použité tooall uživatelé" tooconfirm zpracování se dokončilo.</span><span class="sxs-lookup"><span data-stu-id="d9035-136">Look for “Latest license changes have been applied tooall users" tooconfirm processing has completed.</span></span>

  - <span data-ttu-id="d9035-137">Hledejte v horní části o všechny uživatele, pro které licence pravděpodobně nebyla přiřazena úspěšně oznámení.</span><span class="sxs-lookup"><span data-stu-id="d9035-137">Look for a notification on top about any users for whom licenses may have not been successfully assigned.</span></span> <span data-ttu-id="d9035-138">Jsme dostatek licencí pro některé uživatele?</span><span class="sxs-lookup"><span data-stu-id="d9035-138">Did we run out of licenses for some users?</span></span> <span data-ttu-id="d9035-139">Mají někteří uživatelé konfliktní licence SKU, které brání dědění skupiny licencí?</span><span class="sxs-lookup"><span data-stu-id="d9035-139">Do some users have conflicting license SKUs that prevent them from inheriting group licenses?</span></span>

3. <span data-ttu-id="d9035-140">Zkontrolujte místo tooverify některé uživatele, které mají hello přímé a skupinu licencí použít.</span><span class="sxs-lookup"><span data-stu-id="d9035-140">Spot check some users tooverify that they have both hello direct and group licenses applied.</span></span> <span data-ttu-id="d9035-141">Okno přejděte toohello pro uživatele, vyberte **licence**a zkontrolujte stav hello licencí.</span><span class="sxs-lookup"><span data-stu-id="d9035-141">Go toohello blade for a user, select **Licenses**, and examine hello state of licenses.</span></span>

  - <span data-ttu-id="d9035-142">Toto je hello očekávaný stav uživatele v průběhu migrace:</span><span class="sxs-lookup"><span data-stu-id="d9035-142">This is hello expected user state during migration:</span></span>

      ![Stav očekávané uživatele](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  <span data-ttu-id="d9035-144">Tím se potvrzuje, že tento hello uživatel má přímé a zděděné licencí.</span><span class="sxs-lookup"><span data-stu-id="d9035-144">This confirms that hello user has both direct and inherited licenses.</span></span> <span data-ttu-id="d9035-145">Vidíte, jak **EMS** a **E3** jsou přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="d9035-145">We see that both **EMS** and **E3** are assigned.</span></span>

  - <span data-ttu-id="d9035-146">Vyberte každý licence tooshow podrobnosti o službách hello povolena.</span><span class="sxs-lookup"><span data-stu-id="d9035-146">Select each license tooshow details about hello enabled services.</span></span> <span data-ttu-id="d9035-147">Může to být použité toocheck, pokud hello přímé a skupinu licencí povolit hello přesně stejnou plány služby pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d9035-147">This can be used toocheck if hello direct and group licenses enable exactly hello same service plans for hello user.</span></span>

      ![Zkontrolujte plány služeb](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. <span data-ttu-id="d9035-149">Po potvrzení, že odpovídají direct a skupinu licencí, můžete začít odebrání přímé licence od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9035-149">After confirming that both direct and group licenses are equivalent, you can start removing direct licenses from users.</span></span> <span data-ttu-id="d9035-150">Můžete si otestovat to jejich odebráním pro jednotlivé uživatele portálu hello a spusťte skripty toohave automatizace, který je v hromadné odebrána.</span><span class="sxs-lookup"><span data-stu-id="d9035-150">You can test this by removing them for individual users in hello portal and then run automation scripts toohave them removed in bulk.</span></span> <span data-ttu-id="d9035-151">Tady je příklad stejného uživatele s hello přímé licence odebrat prostřednictvím portálu hello hello.</span><span class="sxs-lookup"><span data-stu-id="d9035-151">Here is an example of hello same user with hello direct licenses removed through hello portal.</span></span> <span data-ttu-id="d9035-152">Všimněte si, že stav licence hello zůstává beze změny, ale už vidíme přímé přiřazení.</span><span class="sxs-lookup"><span data-stu-id="d9035-152">Notice that hello license state remains unchanged, but we no longer see direct assignments.</span></span>

  ![odebrat přímý licencí](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a><span data-ttu-id="d9035-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9035-154">Next steps</span></span>

<span data-ttu-id="d9035-155">Další informace o scénáře pro správu licencí pomocí skupin, přečtěte si toolearn</span><span class="sxs-lookup"><span data-stu-id="d9035-155">toolearn more about other scenarios for license management through groups, read</span></span>

* [<span data-ttu-id="d9035-156">Přiřazení licencí tooa skupiny ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9035-156">Assigning licenses tooa group in Azure Active Directory</span></span>](active-directory-licensing-group-assignment-azure-portal.md)
* [<span data-ttu-id="d9035-157">Co je na základě skupin licencování v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d9035-157">What is group-based licensing in Azure Active Directory?</span></span>](active-directory-licensing-whatis-azure-portal.md)
* [<span data-ttu-id="d9035-158">Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9035-158">Identifying and resolving license problems for a group in Azure Active Directory</span></span>](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [<span data-ttu-id="d9035-159">Azure Active Directory na základě skupin licencí další scénáře</span><span class="sxs-lookup"><span data-stu-id="d9035-159">Azure Active Directory group-based licensing additional scenarios</span></span>](active-directory-licensing-group-advanced.md)
