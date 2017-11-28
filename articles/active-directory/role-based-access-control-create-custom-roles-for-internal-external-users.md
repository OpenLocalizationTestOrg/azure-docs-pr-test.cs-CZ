---
title: "Vytvořte vlastní role řízení přístupu na základě Role a přiřaďte interních a externích uživatelů v Azure | Microsoft Docs"
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
ms.openlocfilehash: d687f94bebfd0b6c1ec0690da798be5409640954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="258d3-103">Úvod na řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="258d3-103">Intro on role-based access control</span></span>

<span data-ttu-id="258d3-104">Řízení přístupu na základě role je Azure portálu pouze funkce povolení vlastníky předplatného přiřadit granulární role jiným uživatelům, kteří mohou spravovat konkrétní prostředek obory ve svém prostředí.</span><span class="sxs-lookup"><span data-stu-id="258d3-104">Role-based access control is an Azure portal only feature allowing the owners of a subscription to assign granular roles to other users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="258d3-105">RBAC umožňuje lepší zabezpečení správy pro velké organizace a pro SMB práce s externími spolupracovníky, dodavatele nebo freelancers, které potřebují přístup ke konkrétním prostředkům ve vašem prostředí, ale nemusí nutně prokázat celé infrastruktury nebo žádné související fakturace obory.</span><span class="sxs-lookup"><span data-stu-id="258d3-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access to specific resources in your environment but not necessarily to the entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="258d3-106">RBAC umožňuje flexibilitu vlastnící jedno předplatné, které spravuje správce účtu (role Správce služby na úrovni předplatného) a pro práci v rámci stejného předplatného, ale bez jakékoli práva správce pro ni pozvali více uživatelů .</span><span class="sxs-lookup"><span data-stu-id="258d3-106">RBAC allows the flexibility of owning one Azure subscription managed by the administrator account (service administrator role at a subscription level) and have multiple users invited to work under the same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="258d3-107">Ze správy a fakturace perspektivy funkci RBAC prokáže, že se možnost efektivní čas a správy pro používání Azure v různých situacích.</span><span class="sxs-lookup"><span data-stu-id="258d3-107">From a management and billing perspective, the RBAC feature proves to be a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="258d3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="258d3-108">Prerequisites</span></span>
<span data-ttu-id="258d3-109">Používání RBAC v prostředí Azure vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="258d3-109">Using RBAC in the Azure environment requires:</span></span>

* <span data-ttu-id="258d3-110">Nutnosti samostatné předplatné přiřazeny uživateli jako vlastník (předplatné role)</span><span class="sxs-lookup"><span data-stu-id="258d3-110">Having a standalone Azure subscription assigned to the user as owner (subscription role)</span></span>
* <span data-ttu-id="258d3-111">Mít roli vlastníka předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="258d3-111">Have the Owner role of the Azure subscription</span></span>
* <span data-ttu-id="258d3-112">K dispozici [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="258d3-112">Have access to the [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="258d3-113">Zajistěte si následující zprostředkovatelé prostředků zaregistrovat pro předplatné uživatele: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="258d3-113">Make sure to have the following Resource Providers registered for the user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="258d3-114">Další informace o postupu při registraci zprostředkovatele prostředků najdete v tématu [zprostředkovatelé Resource Manager, oblastí, verzí rozhraní API a schémat](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="258d3-114">For more information on how to register the resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="258d3-115">Předplatná Office 365 nebo Azure Active Directory licence (například: přístup k Azure Active Directory) zajištěného z O365 portálu není kvality pomocí RBAC.</span><span class="sxs-lookup"><span data-stu-id="258d3-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access to Azure Active Directory) provisioned from the O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="258d3-116">RBAC použití</span><span class="sxs-lookup"><span data-stu-id="258d3-116">How can RBAC be used</span></span>
<span data-ttu-id="258d3-117">RBAC lze použít na tři různé rozsahy v Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="258d3-118">Z oboru nejvyšší nejnižší tomu, že jsou následující:</span><span class="sxs-lookup"><span data-stu-id="258d3-118">From the highest scope to the lowest one, they are as follows:</span></span>

* <span data-ttu-id="258d3-119">Předplatné (nejvyšší)</span><span class="sxs-lookup"><span data-stu-id="258d3-119">Subscription (highest)</span></span>
* <span data-ttu-id="258d3-120">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="258d3-120">Resource group</span></span>
* <span data-ttu-id="258d3-121">Prostředek oboru (nejnižší úroveň přístupu nabídky cílové oprávnění v oboru jednotlivých prostředků Azure)</span><span class="sxs-lookup"><span data-stu-id="258d3-121">Resource scope (the lowest access level offering targeted permissions to an individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-the-subscription-scope"></a><span data-ttu-id="258d3-122">Přiřadit role RBAC v oboru předplatného</span><span class="sxs-lookup"><span data-stu-id="258d3-122">Assign RBAC roles at the subscription scope</span></span>
<span data-ttu-id="258d3-123">Existují dvě běžných příkladů, když RBAC je použít (ale mimo jiné):</span><span class="sxs-lookup"><span data-stu-id="258d3-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="258d3-124">Že externí uživatelé organizací pozvat (není součástí uživatele správce klienta Azure Active Directory) ke správě určitých prostředků nebo celý předplatného</span><span class="sxs-lookup"><span data-stu-id="258d3-124">Having external users from the organizations (not part of the admin user's Azure Active Directory tenant) invited to manage certain resources or the whole subscription</span></span>
* <span data-ttu-id="258d3-125">Práce s uživateli uvnitř organizace (jsou součástí klienta Azure Active Directory uživatele), ale součást různé týmy nebo skupin, které potřebují granulární přístup pro celé předplatné nebo pro určité skupiny prostředků nebo prostředek oborů v prostředí</span><span class="sxs-lookup"><span data-stu-id="258d3-125">Working with users inside the organization (they are part of the user's Azure Active Directory tenant) but part of different teams or groups which need granular access either to the whole subscription or to certain resource groups or resource scopes in the environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="258d3-126">Udělení přístupu na úrovni předplatného pro uživatele mimo Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="258d3-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="258d3-127">Role RBAC lze udělit pouze systémem **vlastníky** předplatného proto uživatel s oprávněními správce musíte být přihlášeni pomocí uživatelského jména, která má tato role předběžně zařazená nebo vytvořil předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-127">RBAC roles can be granted only by **Owners** of the subscription therefore the admin user must be logged with a username which has this role pre-assigned or has created the Azure subscription.</span></span>

<span data-ttu-id="258d3-128">Z portálu Azure po přihlášení jako správce, vyberte možnost "Odběry" a vyberte požadované.</span><span class="sxs-lookup"><span data-stu-id="258d3-128">From the Azure portal, after you sign-in as admin, select “Subscriptions” and chose the desired one.</span></span>
<span data-ttu-id="258d3-129">![okno odběru na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) ve výchozím nastavení, pokud uživatel s oprávněními správce koupil předplatné Azure, uživateli se zobrazí jako **správce účtu**, tím se roli předplatného.</span><span class="sxs-lookup"><span data-stu-id="258d3-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if the admin user has purchased the Azure subscription, the user will show up as **Account Admin**, this being the subscription role.</span></span> <span data-ttu-id="258d3-130">Další informace o rolích předplatné Azure, najdete v části [přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="258d3-130">For more details on the Azure subscription roles, see [Add or change Azure administrator roles that manage the subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="258d3-131">V tomto příkladu uživatel "alflanigan@outlook.com" je **vlastníka** z "Bezplatnou zkušební verzi" předplatné v AAD klienta "Výchozí klienta Azure".</span><span class="sxs-lookup"><span data-stu-id="258d3-131">In this example, the user "alflanigan@outlook.com" is the **Owner** of the "Free Trial" subscription in the AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="258d3-132">Vzhledem k tomu, že je tento uživatel Tvůrce předplatného Azure se počáteční Account Microsoft "Outlook" (Account Microsoft = Outlook, Live atd.) bude výchozí název domény pro všechny uživatele přidán do tohoto klienta **"@alflaniganuoutlook.onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="258d3-132">Since this user is the creator of the Azure subscription with the initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) the default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="258d3-133">Návrh syntaxe nové domény je tvořen uvedení společně název uživatelské jméno a doménu uživatele, který vytvořil klienta a přidání rozšíření **". onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="258d3-133">By design, the syntax of the new domain is formed by putting together the username and domain name of the user who created the tenant and adding the extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="258d3-134">Kromě toho uživatelé můžou přihlásit pomocí vlastního názvu domény v klientovi po přidání a ověření pro nového klienta.</span><span class="sxs-lookup"><span data-stu-id="258d3-134">Furthermore, users can sign-in with a custom domain name in the tenant after adding and verifying it for the new tenant.</span></span> <span data-ttu-id="258d3-135">Další podrobnosti o tom, jak ověřit vlastní název domény v klienta služby Azure Active Directory najdete v tématu [přidání vlastního názvu domény do adresáře](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="258d3-135">For more details on how to verify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name to your directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="258d3-136">V tomto příkladu adresáři "Výchozí klient Azure" obsahuje pouze uživatele s názvem domény "@alflanigan.onmicrosoft.com".</span><span class="sxs-lookup"><span data-stu-id="258d3-136">In this example, the "Default tenant Azure" directory contains only users with the domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="258d3-137">Po výběru předplatného, musíte kliknout na uživatel s oprávněními správce **řízení přístupu (IAM)** a potom **přidat novou roli**.</span><span class="sxs-lookup"><span data-stu-id="258d3-137">After selecting the subscription, the admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![Funkce IAM řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Přidání nového uživatele v IAM funkce řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="258d3-140">Dalším krokem je vybrat role, kterou chcete přiřadit a uživatel, kterému se přiřadí RBAC role.</span><span class="sxs-lookup"><span data-stu-id="258d3-140">The next step is to select the role to be assigned and the user whom the RBAC role will be assigned to.</span></span> <span data-ttu-id="258d3-141">V **Role** rozevírací nabídce Uživatel s oprávněními správce vidí jenom integrovanou RBAC role, které jsou k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-141">In the **Role** dropdown menu the admin user sees only the built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="258d3-142">Podrobné vysvětlení jednotlivých rolí a jejich přiřaditelnými obory, najdete v části [předdefinované role pro řízení přístupu](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="258d3-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="258d3-143">Pak musí přidat e-mailovou adresu externího uživatele, uživatel s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="258d3-143">The admin user then needs to add the email address of the external user.</span></span> <span data-ttu-id="258d3-144">Očekávané chování je externí uživatel není zobrazena v existujícího klienta.</span><span class="sxs-lookup"><span data-stu-id="258d3-144">The expected behavior is for the external user to not show up in the existing tenant.</span></span> <span data-ttu-id="258d3-145">Po pozval externí uživatel zadá budou viditelné v rámci **odběry > řízení přístupu (IAM)** s aktuální uživateli, které jsou přiřazeny role RBAC v obor předplatného.</span><span class="sxs-lookup"><span data-stu-id="258d3-145">After the external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all the current users which are currently assigned an RBAC role at the Subscription scope.</span></span>





![Přidejte oprávnění do nové role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![seznam rolí RBAC na úrovni předplatného](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="258d3-148">Uživatel "chessercarlton@gmail.com" pozval být **vlastníka** pro předplatné "Bezplatnou zkušební verzi".</span><span class="sxs-lookup"><span data-stu-id="258d3-148">The user "chessercarlton@gmail.com" has been invited to be an **Owner** for the “Free Trial” subscription.</span></span> <span data-ttu-id="258d3-149">Po odeslání pozvánky, obdrží externího uživatele potvrzení e-mailu s odkazem k aktivaci.</span><span class="sxs-lookup"><span data-stu-id="258d3-149">After sending the invitation, the external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="258d3-150">![e-mailová pozvánka pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="258d3-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="258d3-151">Probíhá mimo organizaci, nový uživatel nemá žádné existující atributy v adresáři "Výchozí klient Azure".</span><span class="sxs-lookup"><span data-stu-id="258d3-151">Being external to the organization, the new user does not have any existing attributes in the "Default tenant Azure" directory.</span></span> <span data-ttu-id="258d3-152">Budou vytvořeny po externího uživatele poskytl souhlas zaznamenávají v adresáři, který je přidružen k odběru, který byl přiřazen k roli.</span><span class="sxs-lookup"><span data-stu-id="258d3-152">They will be created after the external user has given consent to be recorded in the directory which is associated with the subscription which he has been assigned a role to.</span></span>





![e-mailové zprávě pozvánky pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="258d3-154">Zobrazuje externí uživatel v klientovi Azure Active Directory od této chvíle jako externí uživatel a tato lze zobrazit na portálu Azure i na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="258d3-154">The external user shows in the Azure Active Directory tenant from now on as external user and this can be viewed both in the Azure portal and in the classic portal.</span></span>





![uživatelé okno azure active directory portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![uživatelé okno azure active directory portálu Azure classic](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="258d3-157">V **uživatelé** zobrazení v obou portálů rozpoznal externí uživatele:</span><span class="sxs-lookup"><span data-stu-id="258d3-157">In the **Users** view in both portals the external users can be recognized by:</span></span>

* <span data-ttu-id="258d3-158">Typ vlastní ikonu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="258d3-158">The different icon type in the Azure portal</span></span>
* <span data-ttu-id="258d3-159">Jiné zdrojové bod v portálu classic</span><span class="sxs-lookup"><span data-stu-id="258d3-159">The different sourcing point in the classic portal</span></span>

<span data-ttu-id="258d3-160">Ale udělení **vlastníka** nebo **Přispěvatel** přístup k externím uživatelem v **předplatné** obor, neumožňuje přístup k adresáři uživatele správce, pokud **Globálního správce** to umožňuje.</span><span class="sxs-lookup"><span data-stu-id="258d3-160">However, granting **Owner** or **Contributor** access to an external user at the **Subscription** scope, does not allow the access to the admin user's directory, unless the **Global Admin** allows it.</span></span> <span data-ttu-id="258d3-161">Ve vlastnosti uživatele **typ uživatele** jehož dvě společné parametry, **člen** a **hosta** lze identifikovat.</span><span class="sxs-lookup"><span data-stu-id="258d3-161">In the user proprieties,  the **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="258d3-162">Člen je uživatel, která je registrována v adresáři, zatímco hosta je uživatel vyzván k adresáři z externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="258d3-162">A member is a user which is registered in the directory while a guest is a user invited to the directory from an external source.</span></span> <span data-ttu-id="258d3-163">Další informace najdete v tématu [jak správci Azure Active Directory přidat uživatele spolupráce B2B](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="258d3-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="258d3-164">Ujistěte se, že po zadání přihlašovacích údajů na portálu, externí uživatel vybere správný adresář, který má přihlášení k.</span><span class="sxs-lookup"><span data-stu-id="258d3-164">Make sure that after entering the credentials in the portal, the external user selects the correct directory to sign-in to.</span></span> <span data-ttu-id="258d3-165">Stejný uživatel můžete mít přístup k více adresářů a můžete vybrat některý z nich kliknutím uživatelské jméno v vpravo nahoře na portálu Azure a potom z rozevíracího seznamu vyberte příslušného adresáře.</span><span class="sxs-lookup"><span data-stu-id="258d3-165">The same user can have access to multiple directories and can select either one of  them by clicking the username in the top right-hand side in the Azure portal and then choose the appropriate directory from the dropdown list.</span></span>

<span data-ttu-id="258d3-166">Při se hostovaného v adresáři, externího uživatele můžete spravovat všechny prostředky pro předplatné Azure, ale nemůže získat přístup k adresáři.</span><span class="sxs-lookup"><span data-stu-id="258d3-166">While being a guest in the directory, the external user can manage all resources for the Azure subscription, but can't access the directory.</span></span>





![přístup omezen na portálu Azure azure active directory](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="258d3-168">Azure Active Directory a předplatné Azure, nemají vztah nadřazený podřízený jako ostatní prostředky služby Azure (například: virtuálních počítačů, virtuálních sítí, webové aplikace, úložiště atd.) s předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="258d3-169">Všechny pozdější je vytvořen, spravovat a účtují pod předplatným Azure, zatímco předplatné služby Azure se používá ke správě přístupu ke službě Azure directory.</span><span class="sxs-lookup"><span data-stu-id="258d3-169">All the latter is created, managed and billed under an Azure subscription while an Azure subscription is used to manage the access to an Azure directory.</span></span> <span data-ttu-id="258d3-170">Další podrobnosti najdete v tématu [předplatné jak Azure souvisí s Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="258d3-170">For more details, see [How an Azure subscription is related to Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="258d3-171">Ze všech předdefinovaných rolí RBAC **vlastníka** a **Přispěvatel** nabízejí úplné správy přístup ke všem prostředkům v prostředí, rozdíl, že Přispěvatel nelze vytvářet a odstraňovat nové role RBAC .</span><span class="sxs-lookup"><span data-stu-id="258d3-171">From all the built-in RBAC roles, **Owner** and **Contributor** offer full management access to all resources in the environment, the difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="258d3-172">Mezi integrované role jako **Přispěvatel virtuálních počítačů** nabízejí úplné správy přístup jen k prostředkům uvádí název, bez ohledu na to **skupiny prostředků** během vytváření do.</span><span class="sxs-lookup"><span data-stu-id="258d3-172">The other built-in roles like **Virtual Machine Contributor** offer full management access only to the resources indicated by the name, regardless of the **Resource Group** they are being created into.</span></span>

<span data-ttu-id="258d3-173">Přiřazení předdefinované role RBAC **Přispěvatel virtuálních počítačů** na úrovni předplatného, znamená, že uživatel přiřazenou roli:</span><span class="sxs-lookup"><span data-stu-id="258d3-173">Assigning the built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that the user assigned the role:</span></span>

* <span data-ttu-id="258d3-174">Můžete zobrazit všechny virtuální počítače bez ohledu na to datum jejich nasazení a skupiny prostředků, které jsou součástí</span><span class="sxs-lookup"><span data-stu-id="258d3-174">Can view all virtual machines regardless their deployment date and the resource groups they are part of</span></span>
* <span data-ttu-id="258d3-175">Má úplné správy přístup k virtuálním počítačům v rámci předplatného</span><span class="sxs-lookup"><span data-stu-id="258d3-175">Has full management access to the virtual machines in the subscription</span></span>
* <span data-ttu-id="258d3-176">Nelze zobrazit u jiných typů prostředků v předplatném</span><span class="sxs-lookup"><span data-stu-id="258d3-176">Can't view any other resource types in the subscription</span></span>
* <span data-ttu-id="258d3-177">Všechny změny z hlediska fakturační nemůže pracovat.</span><span class="sxs-lookup"><span data-stu-id="258d3-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="258d3-178">RBAC se portálu Azure pouze funkce, se nebude udělit přístup k portálu classic.</span><span class="sxs-lookup"><span data-stu-id="258d3-178">RBAC being an Azure portal only feature, it doesn't grant access to the classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a><span data-ttu-id="258d3-179">Předdefinovaná role RBAC přiřadit externího uživatele</span><span class="sxs-lookup"><span data-stu-id="258d3-179">Assign a built-in RBAC role to an external user</span></span>
<span data-ttu-id="258d3-180">Pro různé scénáře v tomto testu, externí uživatele "alflanigan@gmail.com" se přidá jako **Přispěvatel virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="258d3-180">For a different scenario in this test, the external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![předdefinované role Přispěvatel virtuálního počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="258d3-182">Normální chování pro tento externí uživatele s tato předdefinovaná role je chcete zobrazit a spravovat pouze virtuální počítače a jejich přiléhající Resource Manager pouze prostředky potřebné při nasazování.</span><span class="sxs-lookup"><span data-stu-id="258d3-182">The normal behavior for this external user with this built-in role is to see and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="258d3-183">Podle návrhu, nabízí tyto role omezený přístup jenom k jejich příslušné prostředky, které jsou vytvořené na portálu Azure, bez ohledu na to některé můžete stále nasadit na klasickém portálu (například: virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="258d3-183">By design, these limited roles offer access only to their correspondent resources created in the Azure portal, regardless some can still be deployed in the classic portal as well (for example: virtual machines).</span></span>





![Přehled role Přispěvatel virtuálních počítačů na portálu azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a><span data-ttu-id="258d3-185">Udělení přístupu na úrovni předplatného pro uživatele ve stejném adresáři</span><span class="sxs-lookup"><span data-stu-id="258d3-185">Grant access at a subscription level for a user in the same directory</span></span>
<span data-ttu-id="258d3-186">Tok procesu je stejný jako při přidávání externího uživatele, z pohledu správce udělení RBAC role, jakož i uživatele i udělení přístupu k roli.</span><span class="sxs-lookup"><span data-stu-id="258d3-186">The process flow is identical to adding an external user, both from the admin perspective granting the RBAC role as well as the user being granted access to the role.</span></span> <span data-ttu-id="258d3-187">Rozdíl je, že pozvané uživatele neobdrží žádné pozvánek e-mailu jako všechny obory prostředků v rámci předplatného. bude k dispozici v řídicím panelu po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="258d3-187">The difference here is that the invited user will not receive any email invitations as all the resource scopes within the subscription will be available in the dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a><span data-ttu-id="258d3-188">Přiřazení role RBAC v oboru skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="258d3-188">Assign RBAC roles at the resource group scope</span></span>
<span data-ttu-id="258d3-189">Přiřazení na role RBAC **skupiny prostředků** oboru má identické proces pro přiřazení role na úrovni předplatného, pro oba typy uživatelů - externí nebo interní (součást stejný adresář).</span><span class="sxs-lookup"><span data-stu-id="258d3-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning the role at the subscription level, for both types of users - either external or internal (part of the same directory).</span></span> <span data-ttu-id="258d3-190">Uživatelé, které jsou přiřazeny RBAC role je zobrazíte ve svém prostředí pouze skupinu prostředků mají přiřazený přístup z **skupiny prostředků** ikonu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-190">The users which are assigned the RBAC role is to see in their environment only the resource group they have been assigned access from the **Resource Groups** icon in the Azure portal.</span></span>

## <a name="assign-rbac-roles-at-the-resource-scope"></a><span data-ttu-id="258d3-191">Přiřadit role RBAC v oboru prostředků</span><span class="sxs-lookup"><span data-stu-id="258d3-191">Assign RBAC roles at the resource scope</span></span>
<span data-ttu-id="258d3-192">Přiřazení role RBAC v oboru prostředků v Azure má identické proces pro přiřazení role na úrovni předplatného nebo na úrovni skupiny prostředků, následující témže pracovním postupu pro oba scénáře.</span><span class="sxs-lookup"><span data-stu-id="258d3-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning the role at the subscription level or at the resource group level, following the same workflow for both scenarios.</span></span> <span data-ttu-id="258d3-193">Znovu, můžete uživatele, pro které jsou přiřazené RBAC role zobrazení pouze těch položek, které mají přiřazený přístup k, buď v **všechny prostředky** kartě nebo přímo v jejich řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="258d3-193">Again, the users which are assigned the RBAC role can see only the items that they have been assigned access to, either in the **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="258d3-194">Pro uživatele k přihlášení k adresáři správné zajistit je důležitým aspektem pro RBAC jak v oboru skupiny prostředků nebo prostředek oboru.</span><span class="sxs-lookup"><span data-stu-id="258d3-194">An important aspect for RBAC both at resource group scope or resource scope is for the users to make sure to sign-in to the correct directory.</span></span>





![přihlášení Directory na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="258d3-196">Přiřazení role RBAC pro skupinu služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="258d3-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="258d3-197">Všechny scénáře pomocí RBAC na tři různé rozsahy v Azure nabízí oprávnění k správě, nasazení a správa různé prostředky jako uživatel s přiřazenou bez nutnosti správy osobních předplatné.</span><span class="sxs-lookup"><span data-stu-id="258d3-197">All the scenarios using RBAC at the three different scopes in Azure offer the privilege of managing, deploying and administering various resources as an assigned user without the need of managing a personal subscription.</span></span> <span data-ttu-id="258d3-198">Bez ohledu na to je pro předplatné, skupinu prostředků nebo prostředek oboru, všechny prostředky přiřazené uživatelé vytvořili na další se fakturují v rámci jednoho předplatného Azure, kde mají uživatelé přístup k přiřadit RBAC role.</span><span class="sxs-lookup"><span data-stu-id="258d3-198">Regardless the RBAC role is assigned for a subscription, resource group or resource scope, all the resources created further on by the assigned users are billed under the one Azure subscription where the users have access to.</span></span> <span data-ttu-id="258d3-199">Tímto způsobem, uživatelů, kteří mají oprávnění správce pro toto předplatné celý Azure fakturace má úplný přehled o spotřebě, bez ohledu na to kdo spravuje prostředky.</span><span class="sxs-lookup"><span data-stu-id="258d3-199">This way, the users who have billing administrator permissions for that entire Azure subscription has a complete overview on the consumption, regardless who is managing the resources.</span></span>

<span data-ttu-id="258d3-200">Stejným způsobem jako pro skupiny Azure Active Directory s perspektivy, že uživatel s oprávněními správce chce zajistit granulární přístup pro týmy nebo celý oddělení, není jednotlivě pro každého uživatele, takže vzhledem k tomu lze použít pro větší organizace role RBAC jej jako velmi čas a správu efektivní možnost.</span><span class="sxs-lookup"><span data-stu-id="258d3-200">For larger organizations, RBAC roles can be applied in the same way for Azure Active Directory groups considering the perspective that the admin user wants to grant the granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="258d3-201">Pro ilustraci v tomto příkladu **Přispěvatel** role je přidaný do jedné ze skupin v klientovi na úrovni předplatného.</span><span class="sxs-lookup"><span data-stu-id="258d3-201">To illustrate this example, the **Contributor** role has been added to one of the groups in the tenant at the subscription level.</span></span>





![Přidání role RBAC pro skupiny AAD](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="258d3-203">Tyto skupiny jsou skupiny zabezpečení, které jsou zřizovat a spravovat pouze v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="258d3-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a><span data-ttu-id="258d3-204">Vytvořit vlastní role RBAC otevření žádosti o podporu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="258d3-204">Create a custom RBAC role to open support requests using PowerShell</span></span>
<span data-ttu-id="258d3-205">Předdefinované role RBAC, které jsou k dispozici v Azure zkontrolujte určité úrovně oprávnění na základě dostupných prostředků v prostředí.</span><span class="sxs-lookup"><span data-stu-id="258d3-205">The built-in RBAC roles which are available in Azure ensure certain permission levels based on the available resources in the environment.</span></span> <span data-ttu-id="258d3-206">Pokud žádná z těchto rolí potřebám Správce uživatelů, existuje však možnost omezit přístup i další vytvořením vlastní role RBAC.</span><span class="sxs-lookup"><span data-stu-id="258d3-206">However, if none of these roles suit the admin user's needs, there is the option to limit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="258d3-207">Vytvoření vlastní role RBAC vyžaduje trvat jednu předdefinovaná role, upravovat a importujte ji zpět do prostředí.</span><span class="sxs-lookup"><span data-stu-id="258d3-207">Creating custom RBAC roles requires to take one built-in role, edit it and then import it back in the environment.</span></span> <span data-ttu-id="258d3-208">Stažení a nahrání role se spravují pomocí prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="258d3-208">The download and upload of the role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="258d3-209">Je důležité pochopit požadavky vytváření vlastní roli, které můžete udělit granulární přístup na úrovni předplatného a taky umožnit pozvané uživatele možnost otevření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="258d3-209">It is important to understand the prerequisites of creating a custom role which can grant granular access at the subscription level and also allow the invited user the flexibility of opening support requests.</span></span>

<span data-ttu-id="258d3-210">V tomto příkladu předdefinovaná role **čtečky** který uživatelům umožňuje přístup k zobrazení všech oborů prostředků, ale nechcete je upravit nebo vytvořit nové byl přizpůsoben, aby uživatel povolit možnost otevření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="258d3-210">For this example the built-in role **Reader** which allows users access to view all the resource scopes but not to edit them or create new ones has been customized to allow the user the option of opening support requests.</span></span>

<span data-ttu-id="258d3-211">Je první akcí exportu **čtečky** spustili role musí být dokončena v prostředí PowerShell se zvýšenými oprávněními jako správce.</span><span class="sxs-lookup"><span data-stu-id="258d3-211">The first action of exporting the **Reader** role needs to be completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Snímek obrazovky prostředí PowerShell pro role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="258d3-213">Budete potřebovat k extrakci šablona JSON role.</span><span class="sxs-lookup"><span data-stu-id="258d3-213">Then you need to extract the JSON template of the role.</span></span>





![Šablona JSON pro vlastní role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="258d3-215">Typické role RBAC se skládá ze tří hlavních částí **akce**, **NotActions** a **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="258d3-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="258d3-216">V **akce** části jsou uvedeny všechny operace, které jsou povolené pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="258d3-216">In the **Action** section are listed all the permitted operations for this role.</span></span> <span data-ttu-id="258d3-217">Je důležité si uvědomit, že každá akce je přiřazený od zprostředkovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="258d3-217">It's important to understand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="258d3-218">V takovém případě pro vytvoření lístky žádostí o podporu **Microsoft.Support** poskytovatele prostředků musí být uvedený.</span><span class="sxs-lookup"><span data-stu-id="258d3-218">In this case, for creating support tickets the **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="258d3-219">Abyste mohli zobrazit všech poskytovatelů prostředků k dispozici a registrovaný v rámci vašeho předplatného, můžete použít PowerShell.</span><span class="sxs-lookup"><span data-stu-id="258d3-219">To be able to see all the resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="258d3-220">Kromě toho můžete zkontrolovat všechny dostupné rutin prostředí PowerShell ke správě zprostředkovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="258d3-220">Additionally, you can check for the all the available PowerShell cmdlets to manage the resource providers.</span></span>
    <span data-ttu-id="258d3-221">![Snímek obrazovky prostředí PowerShell pro správu zprostředkovatele prostředků](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="258d3-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="258d3-222">Pokud chcete omezit všechny akce pro konkrétní role RBAC, zprostředkovatelé prostředků jsou uvedeny v části **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="258d3-222">To restrict all the actions for a particular RBAC role, resource providers are listed under the section **NotActions**.</span></span>
<span data-ttu-id="258d3-223">Poslední je povinný, že RBAC role obsahuje explicitní předplatné ID, kde se používá.</span><span class="sxs-lookup"><span data-stu-id="258d3-223">Last, it's mandatory that the RBAC role contains the explicit subscription IDs where it is used.</span></span> <span data-ttu-id="258d3-224">ID předplatného jsou uvedeny v seznamu **AssignableScopes**, jinak bude nebude povolen import role v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="258d3-224">The subscription IDs are listed under the **AssignableScopes**, otherwise you will not be allowed to import the role in your subscription.</span></span>

<span data-ttu-id="258d3-225">Po vytvoření a vlastní nastavení RBAC role, je nutné naimportovat zpátky prostředí.</span><span class="sxs-lookup"><span data-stu-id="258d3-225">After creating and customizing the RBAC role, it needs to be imported back the environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="258d3-226">V tomto příkladu je vlastní název pro tuto roli RBAC "Čtečky podporu lístky úroveň přístupu" uživatel zobrazit vše, co v rámci předplatného a také otevření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="258d3-226">In this example, the custom name for this RBAC role is "Reader support tickets access level" allowing the user to view everything in the subscription and also to open support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="258d3-227">Jsou pouze dvě předdefinované role RBAC povolení akce otevření žádosti o podporu **vlastníka** a **Přispěvatel**.</span><span class="sxs-lookup"><span data-stu-id="258d3-227">The only two built-in RBAC roles allowing the action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="258d3-228">Uživatel nebude moci otevřít žádosti o podporu musí mohl být přiřazena role RBAC pouze v oboru předplatné, všechny žádosti o podporu se vytváří podle předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-228">For a user to be able to open support requests, he must be assigned an RBAC role only at the subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="258d3-229">Tato nová vlastní role byl přiřazen uživateli ze stejného adresáře.</span><span class="sxs-lookup"><span data-stu-id="258d3-229">This new custom role has been assigned to an user from the same directory.</span></span>





![snímek obrazovky vlastní role RBAC importovat na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![snímek obrazovky přiřazení vlastní importované role RBAC pro uživatele ve stejném adresáři](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![snímek obrazovky oprávnění pro vlastní importované role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="258d3-233">V příkladu byla další podrobné zdůraznit omezení této vlastní role RBAC následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="258d3-233">The example has been further detailed to emphasize the limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="258d3-234">Můžete vytvořit nové žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="258d3-234">Can create new support requests</span></span>
* <span data-ttu-id="258d3-235">Nelze vytvořit nové obory prostředků (například: virtuálního počítače)</span><span class="sxs-lookup"><span data-stu-id="258d3-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="258d3-236">Nelze vytvořit nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="258d3-236">Can't create new resource groups</span></span>





![snímek obrazovky vytváření žádosti o podporu vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![snímek obrazovky vlastní role RBAC Nepodařilo se vytvořit virtuální počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![snímek obrazovky vlastní role RBAC nelze vytvořit nové RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a><span data-ttu-id="258d3-240">Vytvořit vlastní role RBAC otevření žádosti o podporu pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="258d3-240">Create a custom RBAC role to open support requests using Azure CLI</span></span>
<span data-ttu-id="258d3-241">Spouštění v Macu a bez nutnosti přístup k prostředí PowerShell, rozhraní příkazového řádku Azure je způsob, jak můžete přejít.</span><span class="sxs-lookup"><span data-stu-id="258d3-241">Running on a Mac and without having access to PowerShell, Azure CLI is the way to go.</span></span>

<span data-ttu-id="258d3-242">Postup vytvoření vlastních rolí jsou stejné, s jedinou výjimku, která pomocí rozhraní příkazového řádku roli nelze stáhnout v šabloně JSON, ale lze ji zobrazit v rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="258d3-242">The steps to create a custom role are the same, with the sole exception that using CLI the role can't be downloaded in a JSON template, but it can be viewed in the CLI.</span></span>

<span data-ttu-id="258d3-243">V tomto příkladu I vybrali integrované role **zálohování čtečky**.</span><span class="sxs-lookup"><span data-stu-id="258d3-243">For this example I have chosen the built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Snímek obrazovky rozhraní příkazového řádku zálohování čtečky role zobrazit](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="258d3-245">Úpravy roli v sadě Visual Studio po kopírování vlastnosti v šabloně JSON **Microsoft.Support** poskytovatele prostředků byla přidána do **akce** částech tak, aby tento uživatel může otevřít podpory požadavky můžete nadále být čtečku pro trezorů záloh.</span><span class="sxs-lookup"><span data-stu-id="258d3-245">Editing the role in Visual Studio after copying the proprieties in a JSON template, the **Microsoft.Support** resource provider has been added in the **Actions** sections so that this user can open support requests while continuing to be a reader for the backup vaults.</span></span> <span data-ttu-id="258d3-246">Akci je potřeba přidat kde tato role se použije v ID předplatného **AssignableScopes** části.</span><span class="sxs-lookup"><span data-stu-id="258d3-246">Again it is necessary to add the subscription ID where this role will be used in the **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![Rozhraní příkazového řádku snímek obrazovky importování vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="258d3-248">Nová role je nyní k dispozici na webu Azure portal a proces assignation je stejné jako v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="258d3-248">The new role is now available in the Azure portal and the assignation process is the same as in the previous examples.</span></span>





![Azure portálu snímek obrazovky vlastní role RBAC vytvořené pomocí rozhraní příkazového řádku 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="258d3-250">Od verze nejnovější 2017 sestavení je všeobecně dostupná prostředí cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-250">As of the latest Build 2017, the Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="258d3-251">Prostředí Azure Cloud je doplněk k IDE a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="258d3-251">Azure Cloud Shell is a complement to IDE and the Azure Portal.</span></span> <span data-ttu-id="258d3-252">S touto službou můžete získat prostředí založené na prohlížeči, který je ověřen a je hostovaná v Azure a můžete ji použít místo rozhraní příkazového řádku v počítači nainstalován.</span><span class="sxs-lookup"><span data-stu-id="258d3-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Prostředí cloudu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
