---
title: "aaaCreate vlastní role řízení přístupu na základě Role a přiřaďte toointernal a externí uživatele v Azure | Microsoft Docs"
description: "Přiřadit vlastní role RBAC vytvořené pomocí prostředí PowerShell a rozhraní příkazového řádku pro interních a externích uživatelů"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="760fd-103">Úvod na řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="760fd-103">Intro on role-based access control</span></span>

<span data-ttu-id="760fd-104">Řízení přístupu na základě role je Azure portálu pouze funkce umožňuje hello vlastníky předplatného tooassign granulární role tooother uživatelům, kteří mohou spravovat konkrétní prostředek obory ve svém prostředí.</span><span class="sxs-lookup"><span data-stu-id="760fd-104">Role-based access control is an Azure portal only feature allowing hello owners of a subscription tooassign granular roles tooother users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="760fd-105">RBAC umožňuje lepší zabezpečení správy pro velké organizace a pro SMB práce s externími spolupracovníky, dodavatele nebo freelancers, které potřebují přístup k prostředkům toospecific v prostředí, ale nemusí nutně jít toohello celé infrastruktury nebo žádné související fakturace obory.</span><span class="sxs-lookup"><span data-stu-id="760fd-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access toospecific resources in your environment but not necessarily toohello entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="760fd-106">RBAC umožňuje flexibilitu hello vlastnící jedno předplatné spravuje hello správce účtu (role Správce služby na úrovni předplatného) a mít více uživatelů pozvat toowork pod hello stejného předplatného. ale bez všechny správce práva pro něj.</span><span class="sxs-lookup"><span data-stu-id="760fd-106">RBAC allows hello flexibility of owning one Azure subscription managed by hello administrator account (service administrator role at a subscription level) and have multiple users invited toowork under hello same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="760fd-107">Ze správy a fakturace perspektivy funkce RBAC hello prokáže toobe možnost efektivní čas a správy pro používání Azure v různých situacích.</span><span class="sxs-lookup"><span data-stu-id="760fd-107">From a management and billing perspective, hello RBAC feature proves toobe a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="760fd-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="760fd-108">Prerequisites</span></span>
<span data-ttu-id="760fd-109">Používání RBAC v hello prostředí Azure vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="760fd-109">Using RBAC in hello Azure environment requires:</span></span>

* <span data-ttu-id="760fd-110">Nutnosti samostatné předplatné přiřazený uživatel toohello jako vlastník (předplatné role)</span><span class="sxs-lookup"><span data-stu-id="760fd-110">Having a standalone Azure subscription assigned toohello user as owner (subscription role)</span></span>
* <span data-ttu-id="760fd-111">Mít roli vlastníka hello hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="760fd-111">Have hello Owner role of hello Azure subscription</span></span>
* <span data-ttu-id="760fd-112">Mít přístup toohello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="760fd-112">Have access toohello [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="760fd-113">Zajistěte, aby registroval toohave hello následující zprostředkovatelé prostředků pro předplatné uživatele hello: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="760fd-113">Make sure toohave hello following Resource Providers registered for hello user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="760fd-114">Další informace o tom, jak tooregister hello zprostředkovatelé prostředků najdete v tématu [zprostředkovatelé Resource Manager, oblastí, verzí rozhraní API a schémat](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="760fd-114">For more information on how tooregister hello resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="760fd-115">Předplatná Office 365 nebo Azure Active Directory licence (například: přístup k tooAzure služby Active Directory) zajištěného z portálu není kvality pomocí RBAC hello O365.</span><span class="sxs-lookup"><span data-stu-id="760fd-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access tooAzure Active Directory) provisioned from hello O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="760fd-116">RBAC použití</span><span class="sxs-lookup"><span data-stu-id="760fd-116">How can RBAC be used</span></span>
<span data-ttu-id="760fd-117">RBAC lze použít na tři různé rozsahy v Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="760fd-118">Z hello nejvyšší oboru toohello nejnižší jeden ty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="760fd-118">From hello highest scope toohello lowest one, they are as follows:</span></span>

* <span data-ttu-id="760fd-119">Předplatné (nejvyšší)</span><span class="sxs-lookup"><span data-stu-id="760fd-119">Subscription (highest)</span></span>
* <span data-ttu-id="760fd-120">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="760fd-120">Resource group</span></span>
* <span data-ttu-id="760fd-121">Prostředek oboru (hello nejnižší úroveň přístupu nabídky cílové oprávnění oboru tooan jednotlivých prostředků Azure)</span><span class="sxs-lookup"><span data-stu-id="760fd-121">Resource scope (hello lowest access level offering targeted permissions tooan individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a><span data-ttu-id="760fd-122">Přiřadit role RBAC v oboru předplatné hello</span><span class="sxs-lookup"><span data-stu-id="760fd-122">Assign RBAC roles at hello subscription scope</span></span>
<span data-ttu-id="760fd-123">Existují dvě běžných příkladů, když RBAC je použít (ale mimo jiné):</span><span class="sxs-lookup"><span data-stu-id="760fd-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="760fd-124">Že externí uživatelé z organizací hello (není součástí klienta Azure Active Directory uživatele správce hello) pozvat toomanage určité prostředků nebo předplatného celou hello</span><span class="sxs-lookup"><span data-stu-id="760fd-124">Having external users from hello organizations (not part of hello admin user's Azure Active Directory tenant) invited toomanage certain resources or hello whole subscription</span></span>
* <span data-ttu-id="760fd-125">Práce s uživateli uvnitř hello organizace (jsou součástí klienta Azure Active Directory hello uživatele), ale součást různé týmy nebo skupin, které potřebují přístup granulární buď toohello celé předplatné nebo toocertain skupiny prostředků nebo prostředek rozsahy hello prostředí</span><span class="sxs-lookup"><span data-stu-id="760fd-125">Working with users inside hello organization (they are part of hello user's Azure Active Directory tenant) but part of different teams or groups which need granular access either toohello whole subscription or toocertain resource groups or resource scopes in hello environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="760fd-126">Udělení přístupu na úrovni předplatného pro uživatele mimo Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="760fd-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="760fd-127">Role RBAC lze udělit pouze systémem **vlastníky** hello předplatného proto hello správce musí být přihlášený uživatel s uživatelským jménem, které má tato role předběžně zařazená nebo vytvořil hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-127">RBAC roles can be granted only by **Owners** of hello subscription therefore hello admin user must be logged with a username which has this role pre-assigned or has created hello Azure subscription.</span></span>

<span data-ttu-id="760fd-128">Z hello portálu Azure po jste přihlášení jako správce, vyberte možnost "Odběry" a vyberte hello požadované jeden.</span><span class="sxs-lookup"><span data-stu-id="760fd-128">From hello Azure portal, after you sign-in as admin, select “Subscriptions” and chose hello desired one.</span></span>
<span data-ttu-id="760fd-129">![okno odběru na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) ve výchozím nastavení, pokud uživatel s oprávněními správce hello zakoupila hello předplatné Azure, hello uživatele se zobrazí jako **správce účtu**, tato se hello předplatné role.</span><span class="sxs-lookup"><span data-stu-id="760fd-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if hello admin user has purchased hello Azure subscription, hello user will show up as **Account Admin**, this being hello subscription role.</span></span> <span data-ttu-id="760fd-130">Další informace o hello předplatného Azure rolí, najdete v části [přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="760fd-130">For more details on hello Azure subscription roles, see [Add or change Azure administrator roles that manage hello subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="760fd-131">V tomto příkladu hello uživatele "alflanigan@outlook.com" je hello **vlastníka** z hello "Bezplatnou zkušební verzi" předplatné v hello AAD klienta "Výchozí klienta Azure".</span><span class="sxs-lookup"><span data-stu-id="760fd-131">In this example, hello user "alflanigan@outlook.com" is hello **Owner** of hello "Free Trial" subscription in hello AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="760fd-132">Vzhledem k tomu, že je tento uživatel hello Tvůrce hello předplatného Azure s hello počáteční Account Microsoft "Outlook" (Account Microsoft = Outlook, Live atd.) bude hello výchozí název domény pro všechny uživatele přidán do tohoto klienta **"@alflaniganuoutlook.onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="760fd-132">Since this user is hello creator of hello Azure subscription with hello initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) hello default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="760fd-133">Standardně je vytvořen hello syntaxe hello nové domény umístěním společně hello uživatelské jméno a doménu název hello uživatele, který vytvořil hello klienta a přidání rozšíření hello **". onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="760fd-133">By design, hello syntax of hello new domain is formed by putting together hello username and domain name of hello user who created hello tenant and adding hello extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="760fd-134">Kromě toho uživatelé můžou přihlásit pomocí vlastního názvu domény v klientovi hello po přidání a ověření pro nového klienta hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-134">Furthermore, users can sign-in with a custom domain name in hello tenant after adding and verifying it for hello new tenant.</span></span> <span data-ttu-id="760fd-135">Další informace o tom, jak tooverify vlastního názvu domény v klienta služby Azure Active Directory najdete v části [přidat adresář tooyour název vlastní domény](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="760fd-135">For more details on how tooverify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name tooyour directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="760fd-136">V tomto příkladu hello "výchozí klient Azure" adresář obsahuje pouze uživatelé s názvem domény hello "@alflanigan.onmicrosoft.com".</span><span class="sxs-lookup"><span data-stu-id="760fd-136">In this example, hello "Default tenant Azure" directory contains only users with hello domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="760fd-137">Po výběru předplatného hello, musíte kliknout na uživatel s oprávněními správce hello **řízení přístupu (IAM)** a potom **přidat novou roli**.</span><span class="sxs-lookup"><span data-stu-id="760fd-137">After selecting hello subscription, hello admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![Funkce IAM řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Přidání nového uživatele v IAM funkce řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="760fd-140">dalším krokem Hello je tooselect hello role toobe přiřazené a hello uživatele, kterému se přiřadí hello RBAC role.</span><span class="sxs-lookup"><span data-stu-id="760fd-140">hello next step is tooselect hello role toobe assigned and hello user whom hello RBAC role will be assigned to.</span></span> <span data-ttu-id="760fd-141">V hello **Role** rozevírací nabídce hello správce uživateli se zobrazí pouze hello předdefinované RBAC role, které jsou k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-141">In hello **Role** dropdown menu hello admin user sees only hello built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="760fd-142">Podrobné vysvětlení jednotlivých rolí a jejich přiřaditelnými obory, najdete v části [předdefinované role pro řízení přístupu](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="760fd-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="760fd-143">uživatel s oprávněními správce Hello pak musí tooadd hello e-mailovou adresu externího uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-143">hello admin user then needs tooadd hello email address of hello external user.</span></span> <span data-ttu-id="760fd-144">Hello očekává, že je chování pro hello externího uživatele toonot se zobrazí v hello existující klienta.</span><span class="sxs-lookup"><span data-stu-id="760fd-144">hello expected behavior is for hello external user toonot show up in hello existing tenant.</span></span> <span data-ttu-id="760fd-145">Po pozval hello externího uživatele, mohl se nebude zobrazovat v části **odběry > řízení přístupu (IAM)** se všemi uživateli aktuální hello, které jsou přiřazeny role RBAC v hello obor předplatného.</span><span class="sxs-lookup"><span data-stu-id="760fd-145">After hello external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all hello current users which are currently assigned an RBAC role at hello Subscription scope.</span></span>





![Přidání role RBAC toonew oprávnění](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![seznam rolí RBAC na úrovni předplatného](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="760fd-148">uživatel Hello "chessercarlton@gmail.com" byl pozvané toobe **vlastníka** pro hello "Bezplatnou zkušební verzi" předplatného.</span><span class="sxs-lookup"><span data-stu-id="760fd-148">hello user "chessercarlton@gmail.com" has been invited toobe an **Owner** for hello “Free Trial” subscription.</span></span> <span data-ttu-id="760fd-149">Po odeslání pozvánky hello, obdrží hello externího uživatele potvrzení e-mailu s odkazem k aktivaci.</span><span class="sxs-lookup"><span data-stu-id="760fd-149">After sending hello invitation, hello external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="760fd-150">![e-mailová pozvánka pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="760fd-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="760fd-151">Probíhá externí toohello organizace, hello nový uživatel nemá žádné existující atributy v adresáři "Výchozí klienta Azure" hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-151">Being external toohello organization, hello new user does not have any existing attributes in hello "Default tenant Azure" directory.</span></span> <span data-ttu-id="760fd-152">Bude vytvořen po externího uživatele hello udělil souhlas toobe zaznamenaná v hello adresář, který je přidružen ke hello odběr, který byl přiřazen k roli.</span><span class="sxs-lookup"><span data-stu-id="760fd-152">They will be created after hello external user has given consent toobe recorded in hello directory which is associated with hello subscription which he has been assigned a role to.</span></span>





![e-mailové zprávě pozvánky pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="760fd-154">Hello externího uživatele zobrazuje v hello klienta Azure Active Directory od této chvíle jako externí uživatel a tento lze zobrazit v hello portálu Azure i na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-154">hello external user shows in hello Azure Active Directory tenant from now on as external user and this can be viewed both in hello Azure portal and in hello classic portal.</span></span>





![uživatelé okno azure active directory portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![uživatelé okno azure active directory portálu Azure classic](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="760fd-157">V hello **uživatelé** rozpoznal zobrazení v obou portálů hello externích uživatelů:</span><span class="sxs-lookup"><span data-stu-id="760fd-157">In hello **Users** view in both portals hello external users can be recognized by:</span></span>

* <span data-ttu-id="760fd-158">Zadejte jinou ikonu Hello hello portál Azure</span><span class="sxs-lookup"><span data-stu-id="760fd-158">hello different icon type in hello Azure portal</span></span>
* <span data-ttu-id="760fd-159">Hello různých sourcing bodu portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="760fd-159">hello different sourcing point in hello classic portal</span></span>

<span data-ttu-id="760fd-160">Ale udělení **vlastníka** nebo **Přispěvatel** přístup tooan externí uživatel v hello **předplatné** obor, nepovoluje hello přístup toohello správce adresáře uživatele, Pokud hello **globálního správce** to umožňuje.</span><span class="sxs-lookup"><span data-stu-id="760fd-160">However, granting **Owner** or **Contributor** access tooan external user at hello **Subscription** scope, does not allow hello access toohello admin user's directory, unless hello **Global Admin** allows it.</span></span> <span data-ttu-id="760fd-161">V hello vlastnosti uživatele, hello **typ uživatele** jehož dvě společné parametry, **člen** a **hosta** lze identifikovat.</span><span class="sxs-lookup"><span data-stu-id="760fd-161">In hello user proprieties,  hello **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="760fd-162">Člen je uživatel, která je registrována v adresáři hello při hosta toohello adresáře pozvat uživatele z externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="760fd-162">A member is a user which is registered in hello directory while a guest is a user invited toohello directory from an external source.</span></span> <span data-ttu-id="760fd-163">Další informace najdete v tématu [jak správci Azure Active Directory přidat uživatele spolupráce B2B](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="760fd-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="760fd-164">Ujistěte se, že po zadání přihlašovacích údajů hello hello portálu, hello externí uživatel vybere hello toosign ve správném adresáři.</span><span class="sxs-lookup"><span data-stu-id="760fd-164">Make sure that after entering hello credentials in hello portal, hello external user selects hello correct directory toosign-in to.</span></span> <span data-ttu-id="760fd-165">Hello stejného uživatele můžete mít přístup toomultiple adresáře a můžete vybrat buď jeden z nich kliknutím hello uživatelské jméno v hello vpravo nahoře v hello portál Azure a pak vyberte příslušný adresář hello z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-165">hello same user can have access toomultiple directories and can select either one of  them by clicking hello username in hello top right-hand side in hello Azure portal and then choose hello appropriate directory from hello dropdown list.</span></span>

<span data-ttu-id="760fd-166">Při se hostovaného v adresáři hello, hello externího uživatele můžete spravovat všechny prostředky pro hello předplatné Azure, ale hello adresáři nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="760fd-166">While being a guest in hello directory, hello external user can manage all resources for hello Azure subscription, but can't access hello directory.</span></span>





![přístup k portálu Azure active directory s omezeným přístupem tooazure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="760fd-168">Azure Active Directory a předplatné Azure, nemají vztah nadřazený podřízený jako ostatní prostředky služby Azure (například: virtuálních počítačů, virtuálních sítí, webové aplikace, úložiště atd.) s předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="760fd-169">Všechny pozdější hello je vytvořen, spravovat a účtují pod předplatným Azure při předplatné Azure použité toomanage hello přístup tooan Azure directory.</span><span class="sxs-lookup"><span data-stu-id="760fd-169">All hello latter is created, managed and billed under an Azure subscription while an Azure subscription is used toomanage hello access tooan Azure directory.</span></span> <span data-ttu-id="760fd-170">Další podrobnosti najdete v tématu [jak Azure předplatného je související tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="760fd-170">For more details, see [How an Azure subscription is related tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="760fd-171">Ze všech hello předdefinované RBAC rolí **vlastníka** a **Přispěvatel** nabízejí úplné správy přístupu tooall prostředků v prostředí hello, hello rozdíl probíhá, který nelze vytvořit Přispěvatel a odstranění, nové Role RBAC.</span><span class="sxs-lookup"><span data-stu-id="760fd-171">From all hello built-in RBAC roles, **Owner** and **Contributor** offer full management access tooall resources in hello environment, hello difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="760fd-172">Hello jiné předdefinované role, jako **Přispěvatel virtuálních počítačů** nabízejí úplné správy přístup pouze prostředky toohello indikován hello název, bez ohledu na to hello **skupiny prostředků** během vytváření do.</span><span class="sxs-lookup"><span data-stu-id="760fd-172">hello other built-in roles like **Virtual Machine Contributor** offer full management access only toohello resources indicated by hello name, regardless of hello **Resource Group** they are being created into.</span></span>

<span data-ttu-id="760fd-173">Přiřazení hello předdefinované role RBAC z **Přispěvatel virtuálních počítačů** na úrovni předplatného, znamená dané role uživatele přiřazené hello hello:</span><span class="sxs-lookup"><span data-stu-id="760fd-173">Assigning hello built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that hello user assigned hello role:</span></span>

* <span data-ttu-id="760fd-174">Můžete zobrazit všechny virtuální počítače bez ohledu na to jejich nasazení datum a hello skupin prostředků, které jsou součástí</span><span class="sxs-lookup"><span data-stu-id="760fd-174">Can view all virtual machines regardless their deployment date and hello resource groups they are part of</span></span>
* <span data-ttu-id="760fd-175">Má úplné správy přístupu toohello virtuální počítače v rámci předplatného hello</span><span class="sxs-lookup"><span data-stu-id="760fd-175">Has full management access toohello virtual machines in hello subscription</span></span>
* <span data-ttu-id="760fd-176">Nelze zobrazit u jiných typů prostředků v předplatném hello</span><span class="sxs-lookup"><span data-stu-id="760fd-176">Can't view any other resource types in hello subscription</span></span>
* <span data-ttu-id="760fd-177">Všechny změny z hlediska fakturační nemůže pracovat.</span><span class="sxs-lookup"><span data-stu-id="760fd-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="760fd-178">Probíhá Azure portálu pouze funkce RBAC, ho neuděluje portálu classic toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="760fd-178">RBAC being an Azure portal only feature, it doesn't grant access toohello classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a><span data-ttu-id="760fd-179">Přiřazení předdefinované RBAC role tooan externího uživatele</span><span class="sxs-lookup"><span data-stu-id="760fd-179">Assign a built-in RBAC role tooan external user</span></span>
<span data-ttu-id="760fd-180">Pro jiný scénář v tomto testu, hello externí uživatele "alflanigan@gmail.com" se přidá jako **Přispěvatel virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="760fd-180">For a different scenario in this test, hello external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![předdefinované role Přispěvatel virtuálního počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="760fd-182">Hello normální chování pro tento externí uživatele s tato předdefinovaná role je toosee a spravovat pouze virtuální počítače a jejich přiléhající Resource Manager pouze prostředky potřebné při nasazování.</span><span class="sxs-lookup"><span data-stu-id="760fd-182">hello normal behavior for this external user with this built-in role is toosee and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="760fd-183">Návrh těchto omezené rolí nabízí přístup jenom příslušné prostředky tootheir vytvořené v hello portálu Azure, bez ohledu na to některé je stále možné nasadit v hello klasického portálu (například: virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="760fd-183">By design, these limited roles offer access only tootheir correspondent resources created in hello Azure portal, regardless some can still be deployed in hello classic portal as well (for example: virtual machines).</span></span>





![Přehled role Přispěvatel virtuálních počítačů na portálu azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a><span data-ttu-id="760fd-185">Udělení přístupu na úrovni předplatného pro uživatele v hello stejný adresář</span><span class="sxs-lookup"><span data-stu-id="760fd-185">Grant access at a subscription level for a user in hello same directory</span></span>
<span data-ttu-id="760fd-186">tok procesu Hello je identické tooadding externího uživatele, jak z hello perspektivy poskytující hello RBAC role správce, jakož i udělení přístupu toohello role uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-186">hello process flow is identical tooadding an external user, both from hello admin perspective granting hello RBAC role as well as hello user being granted access toohello role.</span></span> <span data-ttu-id="760fd-187">Hello rozdíl je, že uživatel hello pozvat neobdrží žádné pozvánek e-mailu jako všechny obory hello prostředků v rámci předplatného hello bude k dispozici v řídicím panelu hello po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="760fd-187">hello difference here is that hello invited user will not receive any email invitations as all hello resource scopes within hello subscription will be available in hello dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a><span data-ttu-id="760fd-188">Přiřazení role RBAC v oboru skupiny prostředků hello</span><span class="sxs-lookup"><span data-stu-id="760fd-188">Assign RBAC roles at hello resource group scope</span></span>
<span data-ttu-id="760fd-189">Přiřazení role RBAC v **skupiny prostředků** oboru má identické proces pro přiřazení role hello na úrovni předplatného hello, pro oba typy uživatelů - externí nebo interní (součástí hello stejný adresář).</span><span class="sxs-lookup"><span data-stu-id="760fd-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning hello role at hello subscription level, for both types of users - either external or internal (part of hello same directory).</span></span> <span data-ttu-id="760fd-190">Hello uživatelů, které jsou přiřazeny hello RBAC role je toosee ve svém prostředí pouze skupinu prostředků hello mají přiřazený přístup z hello **skupiny prostředků** ikonu v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-190">hello users which are assigned hello RBAC role is toosee in their environment only hello resource group they have been assigned access from hello **Resource Groups** icon in hello Azure portal.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-scope"></a><span data-ttu-id="760fd-191">Přiřadit role RBAC v oboru prostředků hello</span><span class="sxs-lookup"><span data-stu-id="760fd-191">Assign RBAC roles at hello resource scope</span></span>
<span data-ttu-id="760fd-192">Přiřazení role RBAC v oboru prostředků v Azure má identické proces pro přiřazení role hello na úrovni předplatného hello nebo na úrovni skupiny prostředků hello, následující hello stejný pracovní postup pro oba scénáře.</span><span class="sxs-lookup"><span data-stu-id="760fd-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning hello role at hello subscription level or at hello resource group level, following hello same workflow for both scenarios.</span></span> <span data-ttu-id="760fd-193">Hello uživatelů, které jsou přiřazeny hello RBAC role znovu, uvidí jenom hello položky, že mají přiřazený přístup k buď v hello **všechny prostředky** kartě nebo přímo v jejich řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="760fd-193">Again, hello users which are assigned hello RBAC role can see only hello items that they have been assigned access to, either in hello **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="760fd-194">Je důležitým aspektem pro RBAC jak v oboru skupiny prostředků nebo prostředek oboru pro hello uživatelé toomake toohello opravdu toosign ve správném adresáři.</span><span class="sxs-lookup"><span data-stu-id="760fd-194">An important aspect for RBAC both at resource group scope or resource scope is for hello users toomake sure toosign-in toohello correct directory.</span></span>





![přihlášení Directory na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="760fd-196">Přiřazení role RBAC pro skupinu služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="760fd-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="760fd-197">Všechny scénáře hello pomocí RBAC na tři různé rozsahy hello v nabídka Azure hello oprávnění správě, nasazení a správa různé prostředky jako uživatel s přiřazenou bez hello potřebovat spravovat osobní předplatné.</span><span class="sxs-lookup"><span data-stu-id="760fd-197">All hello scenarios using RBAC at hello three different scopes in Azure offer hello privilege of managing, deploying and administering various resources as an assigned user without hello need of managing a personal subscription.</span></span> <span data-ttu-id="760fd-198">Bez ohledu na to hello RBAC role je přiřazená pro předplatné, skupinu prostředků nebo prostředek oboru, všechny prostředky hello hello přiřazené uživatelé vytvořili na další se fakturují pod hello jedno předplatné v které hello uživatelé mají přístup k.</span><span class="sxs-lookup"><span data-stu-id="760fd-198">Regardless hello RBAC role is assigned for a subscription, resource group or resource scope, all hello resources created further on by hello assigned users are billed under hello one Azure subscription where hello users have access to.</span></span> <span data-ttu-id="760fd-199">Tímto způsobem hello uživatelé, kteří mají oprávnění správce pro toto předplatné celý Azure fakturace má úplný přehled o spotřebě hello, bez ohledu na to, který spravuje prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-199">This way, hello users who have billing administrator permissions for that entire Azure subscription has a complete overview on hello consumption, regardless who is managing hello resources.</span></span>

<span data-ttu-id="760fd-200">Pro větší organizace, můžete použít role RBAC v hello stejným způsobem jako pro skupiny Azure Active Directory s hello perspektivy tento uživatel správce hello chce toogrant hello granulární přístup pro týmy nebo celý oddělení, není jednotlivě pro každého uživatele, tedy vzhledem k tomu, ho jako velmi čas a správu efektivní možnost.</span><span class="sxs-lookup"><span data-stu-id="760fd-200">For larger organizations, RBAC roles can be applied in hello same way for Azure Active Directory groups considering hello perspective that hello admin user wants toogrant hello granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="760fd-201">tooillustrate tento příklad, hello **Přispěvatel** role přidala tooone hello skupin v klientovi hello na úrovni předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-201">tooillustrate this example, hello **Contributor** role has been added tooone of hello groups in hello tenant at hello subscription level.</span></span>





![Přidání role RBAC pro skupiny AAD](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="760fd-203">Tyto skupiny jsou skupiny zabezpečení, které jsou zřizovat a spravovat pouze v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="760fd-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a><span data-ttu-id="760fd-204">Vytvořit vlastní podporu tooopen role RBAC požadavky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="760fd-204">Create a custom RBAC role tooopen support requests using PowerShell</span></span>
<span data-ttu-id="760fd-205">Hello předdefinované RBAC role, které jsou k dispozici v Azure zkontrolujte určité úrovně oprávnění na základě dostupných prostředků hello v prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-205">hello built-in RBAC roles which are available in Azure ensure certain permission levels based on hello available resources in hello environment.</span></span> <span data-ttu-id="760fd-206">Pokud žádná z těchto rolí potřebám hello Správce uživatelů, je však hello možnost toolimit přístup i další vytvořením vlastní role RBAC.</span><span class="sxs-lookup"><span data-stu-id="760fd-206">However, if none of these roles suit hello admin user's needs, there is hello option toolimit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="760fd-207">Vytvoření vlastní role RBAC vyžaduje, aby tootake jeden předdefinovaná role, upravovat a importujte ji zpět do prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-207">Creating custom RBAC roles requires tootake one built-in role, edit it and then import it back in hello environment.</span></span> <span data-ttu-id="760fd-208">stažení Hello a nahrání hello role se spravují pomocí prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="760fd-208">hello download and upload of hello role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="760fd-209">Je důležité toounderstand požadavky hello vytváření vlastní roli, které můžete udělit granulární přístup na úrovni předplatného hello a také dát hello pozvat uživatele hello možnost otevření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="760fd-209">It is important toounderstand hello prerequisites of creating a custom role which can grant granular access at hello subscription level and also allow hello invited user hello flexibility of opening support requests.</span></span>

<span data-ttu-id="760fd-210">Pro tento příklad hello předdefinovaná role **čtečky** což umožňuje uživatelům přístup tooview prostředků všech hello rozsahy ale není tooedit je nebo vytvořit nové byl přizpůsobit tooallow hello uživatele hello možnost otevření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="760fd-210">For this example hello built-in role **Reader** which allows users access tooview all hello resource scopes but not tooedit them or create new ones has been customized tooallow hello user hello option of opening support requests.</span></span>

<span data-ttu-id="760fd-211">Hello první akce exportu hello **čtečky** spustili toobe role musí dokončit v prostředí PowerShell se zvýšenými oprávněními jako správce.</span><span class="sxs-lookup"><span data-stu-id="760fd-211">hello first action of exporting hello **Reader** role needs toobe completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Snímek obrazovky prostředí PowerShell pro role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="760fd-213">Pak musíte tooextract hello JSON šablony role hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-213">Then you need tooextract hello JSON template of hello role.</span></span>





![Šablona JSON pro vlastní role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="760fd-215">Typické role RBAC se skládá ze tří hlavních částí **akce**, **NotActions** a **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="760fd-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="760fd-216">V hello **akce** části jsou uvedeny všechny hello povolených operací pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="760fd-216">In hello **Action** section are listed all hello permitted operations for this role.</span></span> <span data-ttu-id="760fd-217">Je důležité toounderstand přiřazený každá akce od zprostředkovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="760fd-217">It's important toounderstand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="760fd-218">V takovém případě pro vytvoření hello lístků podpory **Microsoft.Support** poskytovatele prostředků musí být uvedený.</span><span class="sxs-lookup"><span data-stu-id="760fd-218">In this case, for creating support tickets hello **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="760fd-219">možnost toosee toobe všechny hello zprostředkovatelé prostředků k dispozici a registrovaný v rámci vašeho předplatného, můžete použít prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="760fd-219">toobe able toosee all hello resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="760fd-220">Kromě toho můžete zobrazit v hello všech hello dostupné prostředí PowerShell rutiny toomanage hello poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="760fd-220">Additionally, you can check for hello all hello available PowerShell cmdlets toomanage hello resource providers.</span></span>
    <span data-ttu-id="760fd-221">![Snímek obrazovky prostředí PowerShell pro správu zprostředkovatele prostředků](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="760fd-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="760fd-222">všechny akce pro konkrétní role RBAC prostředků poskytovatelé jsou uvedené části hello hello toorestrict **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="760fd-222">toorestrict all hello actions for a particular RBAC role, resource providers are listed under hello section **NotActions**.</span></span>
<span data-ttu-id="760fd-223">Poslední je povinný, že tato role RBAC hello obsahuje explicitní předplatné hello ID, kde se používá.</span><span class="sxs-lookup"><span data-stu-id="760fd-223">Last, it's mandatory that hello RBAC role contains hello explicit subscription IDs where it is used.</span></span> <span data-ttu-id="760fd-224">Hello ID předplatného jsou uvedeny v části hello **AssignableScopes**, v opačném případě je nebudou povolena, tooimport hello role v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="760fd-224">hello subscription IDs are listed under hello **AssignableScopes**, otherwise you will not be allowed tooimport hello role in your subscription.</span></span>

<span data-ttu-id="760fd-225">Po vytvoření a vlastní nastavení hello RBAC role, potřebuje toobe importované back hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="760fd-225">After creating and customizing hello RBAC role, it needs toobe imported back hello environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="760fd-226">V tomto příkladu je hello vlastní název této role RBAC "Čtečky podporu lístky úroveň přístupu" povolení hello uživatele tooview všechno, co v hello předplatné a také tooopen žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="760fd-226">In this example, hello custom name for this RBAC role is "Reader support tickets access level" allowing hello user tooview everything in hello subscription and also tooopen support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="760fd-227">jsou technologie Hello pouze dvě předdefinované role RBAC povolení hello akce otevření žádosti o podporu **vlastníka** a **Přispěvatel**.</span><span class="sxs-lookup"><span data-stu-id="760fd-227">hello only two built-in RBAC roles allowing hello action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="760fd-228">Pro podporu požadavky možné tooopen toobe uživatel mohl musí mít přiřazenou RBAC pouze v oboru předplatné hello, všechny žádosti o podporu se vytváří podle předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-228">For a user toobe able tooopen support requests, he must be assigned an RBAC role only at hello subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="760fd-229">Tato nová vlastní role byla přiřazena tooan uživatele z hello stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="760fd-229">This new custom role has been assigned tooan user from hello same directory.</span></span>





![snímek obrazovky vlastní role RBAC naimportována hello portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![snímek obrazovky přiřazení vlastní importované toouser RBAC role v hello stejný adresář](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![snímek obrazovky oprávnění pro vlastní importované role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="760fd-233">Příklad Hello byl další omezení hello podrobné tooemphasize této vlastní role RBAC následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="760fd-233">hello example has been further detailed tooemphasize hello limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="760fd-234">Můžete vytvořit nové žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="760fd-234">Can create new support requests</span></span>
* <span data-ttu-id="760fd-235">Nelze vytvořit nové obory prostředků (například: virtuálního počítače)</span><span class="sxs-lookup"><span data-stu-id="760fd-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="760fd-236">Nelze vytvořit nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="760fd-236">Can't create new resource groups</span></span>





![snímek obrazovky vytváření žádosti o podporu vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![snímek obrazovky vlastní role RBAC nebylo možné toocreate virtuální počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![snímek obrazovky vlastní role RBAC nebylo možné toocreate nové RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a><span data-ttu-id="760fd-240">Vytvořit vlastní podporu tooopen role RBAC požadavků pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="760fd-240">Create a custom RBAC role tooopen support requests using Azure CLI</span></span>
<span data-ttu-id="760fd-241">Spouštění v Macu a bez nutnosti přístupu tooPowerShell, rozhraní příkazového řádku Azure je způsob, jak toogo hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-241">Running on a Mac and without having access tooPowerShell, Azure CLI is hello way toogo.</span></span>

<span data-ttu-id="760fd-242">Hello kroky toocreate vlastní role jsou hello stejné, s jedinou výjimkou hello, pomocí rozhraní příkazového řádku hello role nelze stáhnout v šablonu JSON, ale lze zobrazit v hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="760fd-242">hello steps toocreate a custom role are hello same, with hello sole exception that using CLI hello role can't be downloaded in a JSON template, but it can be viewed in hello CLI.</span></span>

<span data-ttu-id="760fd-243">V tomto příkladu I rozhodli hello předdefinované role **zálohování čtečky**.</span><span class="sxs-lookup"><span data-stu-id="760fd-243">For this example I have chosen hello built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Snímek obrazovky rozhraní příkazového řádku zálohování čtečky role zobrazit](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="760fd-245">Po zkopírování hello vlastnosti v šabloně JSON úpravy hello role v sadě Visual Studio, hello **Microsoft.Support** poskytovatele prostředků byla přidána do hello **akce** částech tak, aby tento uživatel může otevřít žádosti o podporu přitom dál toobe čtečku pro hello záloh.</span><span class="sxs-lookup"><span data-stu-id="760fd-245">Editing hello role in Visual Studio after copying hello proprieties in a JSON template, hello **Microsoft.Support** resource provider has been added in hello **Actions** sections so that this user can open support requests while continuing toobe a reader for hello backup vaults.</span></span> <span data-ttu-id="760fd-246">Znovu případě je nutné tooadd ID předplatného hello kde tato role se použije v hello **AssignableScopes** části.</span><span class="sxs-lookup"><span data-stu-id="760fd-246">Again it is necessary tooadd hello subscription ID where this role will be used in hello **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![Rozhraní příkazového řádku snímek obrazovky importování vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="760fd-248">Nová role Hello je teď dostupná v hello portál Azure a proces assignation hello je hello stejné jako v předchozích příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="760fd-248">hello new role is now available in hello Azure portal and hello assignation process is hello same as in hello previous examples.</span></span>





![Azure portálu snímek obrazovky vlastní role RBAC vytvořené pomocí rozhraní příkazového řádku 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="760fd-250">Od verze hello je všeobecně dostupná nejnovější 2017 sestavení, hello prostředí cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-250">As of hello latest Build 2017, hello Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="760fd-251">Prostředí Azure Cloud je doplňkovým tooIDE a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="760fd-251">Azure Cloud Shell is a complement tooIDE and hello Azure Portal.</span></span> <span data-ttu-id="760fd-252">S touto službou můžete získat prostředí založené na prohlížeči, který je ověřen a je hostovaná v Azure a můžete ji použít místo rozhraní příkazového řádku v počítači nainstalován.</span><span class="sxs-lookup"><span data-stu-id="760fd-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Prostředí cloudu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
