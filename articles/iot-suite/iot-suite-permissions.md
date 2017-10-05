---
title: Azure IoT Suite a Azure Active Directory | Microsoft Docs
description: "Popisuje, jak Azure IoT Suite využívá Azure Active Directory ke správě oprávnění."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 518e6a481ab6385b03dd3ddc2e155fb724e677fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a><span data-ttu-id="743f3-103">Oprávnění na webu azureiotsuite.com</span><span class="sxs-lookup"><span data-stu-id="743f3-103">Permissions on the azureiotsuite.com site</span></span>

## <a name="what-happens-when-you-sign-in"></a><span data-ttu-id="743f3-104">Co se stane, když se přihlásíte</span><span class="sxs-lookup"><span data-stu-id="743f3-104">What happens when you sign in</span></span>

<span data-ttu-id="743f3-105">Přihlaste se na prvním [azureiotsuite.com][lnk-azureiotsuite], lokality Určuje úrovně oprávnění, můžete mít na základě aktuálně vybraného klienta Azure Active Directory (AAD) a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="743f3-105">The first time you sign in at [azureiotsuite.com][lnk-azureiotsuite], the site determines the permission levels you have based on the currently selected Azure Active Directory (AAD) tenant and Azure subscription.</span></span>

1. <span data-ttu-id="743f3-106">Nejprve k naplnění seznamu klientů vidět vedle vaše uživatelské jméno, lokality zjistí z Azure které AAD klienty patří do.</span><span class="sxs-lookup"><span data-stu-id="743f3-106">First, to populate the list of tenants seen next to your username, the site finds out from Azure which AAD tenants you belong to.</span></span> <span data-ttu-id="743f3-107">V současné době lokality může obsahovat pouze tokeny uživatele pro jednoho klienta v čase.</span><span class="sxs-lookup"><span data-stu-id="743f3-107">Currently, the site can only obtain user tokens for one tenant at a time.</span></span> <span data-ttu-id="743f3-108">Proto že při přepnutí klientů pomocí rozevíracího seznamu v pravém horním rohu webu v protokoly na tohoto tenanta získat tokeny pro tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="743f3-108">Therefore, when you switch tenants using the dropdown in the top right corner, the site logs you in to that tenant to obtain the tokens for that tenant.</span></span>

2. <span data-ttu-id="743f3-109">V dalším kroku webu zjistí z Azure, které odběry byla přidružena vybraného klienta.</span><span class="sxs-lookup"><span data-stu-id="743f3-109">Next, the site finds out from Azure which subscriptions you have associated with the selected tenant.</span></span> <span data-ttu-id="743f3-110">Při vytváření nové předkonfigurované řešení zobrazit dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="743f3-110">You see the available subscriptions when you create a new preconfigured solution.</span></span>

3. <span data-ttu-id="743f3-111">Nakonec webu načte všechny prostředky v skupiny prostředků a předplatná označené jako předkonfigurovaná řešení a naplní dlaždice na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="743f3-111">Finally, the site retrieves all the resources in the subscriptions and resource groups tagged as preconfigured solutions and populates the tiles on the home page.</span></span>

<span data-ttu-id="743f3-112">Následující části popisují role, která řídí přístup k předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-112">The following sections describe the roles that control access to the preconfigured solutions.</span></span>

## <a name="aad-roles"></a><span data-ttu-id="743f3-113">Role AAD</span><span class="sxs-lookup"><span data-stu-id="743f3-113">AAD roles</span></span>

<span data-ttu-id="743f3-114">Role AAD zřídit předkonfigurované řešení možnost řídit a spravovat uživatele v předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-114">The AAD roles control the ability provision preconfigured solutions and manage users in a preconfigured solution.</span></span>

<span data-ttu-id="743f3-115">Další informace o rolích správce můžete najít v AAD v [přiřazení rolí správce ve službě Azure AD][lnk-aad-admin].</span><span class="sxs-lookup"><span data-stu-id="743f3-115">You can find more information about administrator roles in AAD in [Assigning administrator roles in Azure AD][lnk-aad-admin].</span></span> <span data-ttu-id="743f3-116">Aktuální článek se týká **globálního správce** a **uživatele** role directory používaného předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-116">The current article focuses on the **Global Administrator** and the **User** directory roles as used by the preconfigured solutions.</span></span>

### <a name="global-administrator"></a><span data-ttu-id="743f3-117">Globální správce</span><span class="sxs-lookup"><span data-stu-id="743f3-117">Global administrator</span></span>

<span data-ttu-id="743f3-118">Může být mnoho globální správci za klienta AAD:</span><span class="sxs-lookup"><span data-stu-id="743f3-118">There can be many global administrators per AAD tenant:</span></span>

* <span data-ttu-id="743f3-119">Při vytváření klienta služby AAD se ve výchozím nastavení globální správce tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="743f3-119">When you create an AAD tenant, you are by default the global administrator of that tenant.</span></span>
* <span data-ttu-id="743f3-120">Globální správce, můžete zřídit předkonfigurované řešení a je mu přiřazená **správce** role pro danou aplikaci uvnitř jejich klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-120">The global administrator can provision a preconfigured solution and is assigned an **Admin** role for the application inside their AAD tenant.</span></span>
* <span data-ttu-id="743f3-121">Pokud byl vytvořen jiným uživatelem ve stejném tenantovi AAD aplikace, je výchozí role Globální správce udělit **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="743f3-121">If another user in the same AAD tenant creates an application, the default role granted to the global administrator is **ReadOnly**.</span></span>
* <span data-ttu-id="743f3-122">Globální správce můžete přiřadit uživatele k rolím pro aplikace pomocí [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="743f3-122">A global administrator can assign users to roles for applications using the [Azure portal][lnk-portal].</span></span>

### <a name="domain-user"></a><span data-ttu-id="743f3-123">Uživatel domény</span><span class="sxs-lookup"><span data-stu-id="743f3-123">Domain user</span></span>

<span data-ttu-id="743f3-124">Může být velký počet uživatelů domény na klienta AAD:</span><span class="sxs-lookup"><span data-stu-id="743f3-124">There can be many domain users per AAD tenant:</span></span>

* <span data-ttu-id="743f3-125">Uživatel domény můžete zřídit předkonfigurované řešení prostřednictvím [azureiotsuite.com] [ lnk-azureiotsuite] lokality.</span><span class="sxs-lookup"><span data-stu-id="743f3-125">A domain user can provision a preconfigured solution through the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="743f3-126">Standardně je povolen uživatele domény **správce** role v aplikaci zřízené.</span><span class="sxs-lookup"><span data-stu-id="743f3-126">By default, the domain user is granted the **Admin** role in the provisioned application.</span></span>
* <span data-ttu-id="743f3-127">Uživatel domény můžete vytvořit aplikaci pomocí skriptu build.cmd [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance][lnk-pm-github-repo], nebo [azure-iot připojené factory] [ lnk-cf-github-repo] úložiště.</span><span class="sxs-lookup"><span data-stu-id="743f3-127">A domain user can create an application using the build.cmd script in the [azure-iot-remote-monitoring][lnk-rm-github-repo],  [azure-iot-predictive-maintenance][lnk-pm-github-repo], or [azure-iot-connected-factory][lnk-cf-github-repo] repository.</span></span> <span data-ttu-id="743f3-128">Je však výchozí role uživatele domény udělit **jen pro čtení**, protože uživatel domény nemá oprávnění k přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="743f3-128">However, the default role granted to the domain user is **ReadOnly**, because a domain user does not have permission to assign roles.</span></span>
* <span data-ttu-id="743f3-129">Pokud byl vytvořen jiným uživatelem v tenantovi AAD aplikace, uživatele domény, je přiřazen **jen pro čtení** roli ve výchozím nastavení pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="743f3-129">If another user in the AAD tenant creates an application, the domain user is assigned the **ReadOnly** role by default for that application.</span></span>
* <span data-ttu-id="743f3-130">Uživatel domény nelze přiřadit role pro aplikace; proto nelze uživatele domény přidat, uživatelé nebo role pro uživatele pro aplikaci i v případě, že se jeho zřízení.</span><span class="sxs-lookup"><span data-stu-id="743f3-130">A domain user cannot assign roles for applications; therefore a domain user cannot add users or roles for users for an application even if they provisioned it.</span></span>

### <a name="guest-user"></a><span data-ttu-id="743f3-131">Uživatele Guest</span><span class="sxs-lookup"><span data-stu-id="743f3-131">Guest User</span></span>

<span data-ttu-id="743f3-132">Může být velký počet uživatelů typu Host za klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-132">There can be many guest users per AAD tenant.</span></span> <span data-ttu-id="743f3-133">Uživatele typu Host mají omezenou sadu oprávnění v tenantovi AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-133">Guest users have a limited set of rights in the AAD tenant.</span></span> <span data-ttu-id="743f3-134">V důsledku toho uživatele typu Host nejde zřídit předkonfigurované řešení v tenantovi AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-134">As a result, guest users cannot provision a preconfigured solution in the AAD tenant.</span></span>

<span data-ttu-id="743f3-135">Další informace týkající se uživatelů a rolí v AAD najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="743f3-135">For more information about users and roles in AAD, see the following resources:</span></span>

* <span data-ttu-id="743f3-136">[Vytvořte uživatele ve službě Azure AD][lnk-create-edit-users]</span><span class="sxs-lookup"><span data-stu-id="743f3-136">[Create users in Azure AD][lnk-create-edit-users]</span></span>
* <span data-ttu-id="743f3-137">[Přiřazení uživatelů k aplikacím][lnk-assign-app-roles]</span><span class="sxs-lookup"><span data-stu-id="743f3-137">[Assign users to apps][lnk-assign-app-roles]</span></span>

## <a name="azure-subscription-administrator-roles"></a><span data-ttu-id="743f3-138">Role správce předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="743f3-138">Azure subscription administrator roles</span></span>

<span data-ttu-id="743f3-139">Role Azure správce řízení možnost mapovat předplatné Azure klient služby AD.</span><span class="sxs-lookup"><span data-stu-id="743f3-139">The Azure admin roles control the ability to map an Azure subscription to an AD tenant.</span></span>

<span data-ttu-id="743f3-140">Další informace o Azure správce rolí v článku [postup přidání nebo změna Azure Spolusprávcem, Správce služeb a správce účtu][lnk-admin-roles].</span><span class="sxs-lookup"><span data-stu-id="743f3-140">Find out more about the Azure admin roles in the article [How to add or change Azure Co-Administrator, Service Administrator, and Account Administrator][lnk-admin-roles].</span></span>

## <a name="application-roles"></a><span data-ttu-id="743f3-141">Aplikační role</span><span class="sxs-lookup"><span data-stu-id="743f3-141">Application roles</span></span>

<span data-ttu-id="743f3-142">Aplikační role řízení přístupu k zařízení v předkonfigurovaném řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-142">The application roles control access to devices in your preconfigured solution.</span></span>

<span data-ttu-id="743f3-143">Existují dvě definované role a jedna implicitní role, které jsou definované v zřízené aplikaci:</span><span class="sxs-lookup"><span data-stu-id="743f3-143">There are two defined roles and one implicit role defined in a provisioned application:</span></span>

* <span data-ttu-id="743f3-144">**Správce:** má plnou kontrolu pro přidání, správu, odeberte zařízení a upravit nastavení.</span><span class="sxs-lookup"><span data-stu-id="743f3-144">**Admin:** Has full control to add, manage, remove devices, and modify settings.</span></span>
* <span data-ttu-id="743f3-145">**Jen pro čtení:** můžete zobrazit zařízení, pravidla, akce, úlohy a telemetrie.</span><span class="sxs-lookup"><span data-stu-id="743f3-145">**ReadOnly:** Can view devices, rules, actions, jobs, and telemetry.</span></span>

<span data-ttu-id="743f3-146">Oprávnění přiřazená uživateli každou roli v můžete najít [RolePermissions.cs] [ lnk-resource-cs] zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="743f3-146">You can find the permissions assigned to each role in the [RolePermissions.cs][lnk-resource-cs] source file.</span></span>

### <a name="changing-application-roles-for-a-user"></a><span data-ttu-id="743f3-147">Změna aplikační role pro uživatele</span><span class="sxs-lookup"><span data-stu-id="743f3-147">Changing application roles for a user</span></span>

<span data-ttu-id="743f3-148">Následující postup vám pomůže nastavit uživatele ve službě Active Directory správcem předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-148">You can use the following procedure to make a user in your Active Directory an administrator of your preconfigured solution.</span></span>

<span data-ttu-id="743f3-149">Musí být globální správce AAD změnit role uživatele:</span><span class="sxs-lookup"><span data-stu-id="743f3-149">You must be an AAD global administrator to change roles for a user:</span></span>

1. <span data-ttu-id="743f3-150">Přejděte na web [Azure Portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="743f3-150">Go to the [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="743f3-151">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="743f3-151">Select **Azure Active Directory**.</span></span>
3. <span data-ttu-id="743f3-152">Ujistěte se, že používáte adresáře, které jste zvolili na azureiotsuite.com při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-152">Make sure you are using the directory you chose on azureiotsuite.com when you provisioned your solution.</span></span> <span data-ttu-id="743f3-153">Pokud máte několik adresářů, které jsou spojené s vaším předplatným, můžete přepnout mezi nimi Pokud kliknete na název účtu v pravé horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="743f3-153">If you have multiple directories associated with your subscription, you can switch between them if you click your account name at the top-right of the portal.</span></span>
4. <span data-ttu-id="743f3-154">Klikněte na tlačítko **podnikové aplikace, které**, pak **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="743f3-154">Click **Enterprise applications**, then **All applications**.</span></span>
4. <span data-ttu-id="743f3-155">Zobrazit **všechny aplikace** s **žádné** stavu.</span><span class="sxs-lookup"><span data-stu-id="743f3-155">Show **All applications** with **Any** status.</span></span> <span data-ttu-id="743f3-156">Poté vyhledejte aplikaci s názvem předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-156">Then search for an application with name of your preconfigured solution.</span></span>
5. <span data-ttu-id="743f3-157">Klikněte na název aplikace, která odpovídá názvu vašeho předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-157">Click the name of the application that matches your preconfigured solution name.</span></span>
6. <span data-ttu-id="743f3-158">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="743f3-158">Click **Users and groups**.</span></span>
7. <span data-ttu-id="743f3-159">Vyberte uživatele, kterého chcete přepnout role.</span><span class="sxs-lookup"><span data-stu-id="743f3-159">Select the user you want to switch roles.</span></span>
8. <span data-ttu-id="743f3-160">Klikněte na tlačítko **přiřadit** a vyberte roli (například **správce**) chcete uživateli přiřadit ručně, klikněte na zatržítko.</span><span class="sxs-lookup"><span data-stu-id="743f3-160">Click **Assign** and select the role (such as **Admin**) you'd like to assign to the user, click the check mark.</span></span>

## <a name="faq"></a><span data-ttu-id="743f3-161">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="743f3-161">FAQ</span></span>

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a><span data-ttu-id="743f3-162">Správce služby a v chcete změnit adresář mapování mezi Moje předplatné a konkrétní klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-162">I'm a service administrator and I'd like to change the directory mapping between my subscription and a specific AAD tenant.</span></span> <span data-ttu-id="743f3-163">Jak tento úkol provést?</span><span class="sxs-lookup"><span data-stu-id="743f3-163">How do I complete this task?</span></span>

1. <span data-ttu-id="743f3-164">Přejděte na [portál Azure classic][lnk-classic-portal], klikněte na tlačítko **nastavení** v seznamu služeb na levé straně.</span><span class="sxs-lookup"><span data-stu-id="743f3-164">Go to the [Azure classic portal][lnk-classic-portal], click **Settings** in the list of services on the left-hand side.</span></span>
2. <span data-ttu-id="743f3-165">Vyberte předplatné, které byste chtěli změnit mapování adresáře na.</span><span class="sxs-lookup"><span data-stu-id="743f3-165">Select the subscription you'd like to change the directory mapping to.</span></span>
3. <span data-ttu-id="743f3-166">Klikněte na tlačítko **upravit adresář**.</span><span class="sxs-lookup"><span data-stu-id="743f3-166">Click **Edit Directory**.</span></span>
4. <span data-ttu-id="743f3-167">Vyberte **Directory** chcete použít v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="743f3-167">Select the **Directory** you would like to use in the dropdown.</span></span> <span data-ttu-id="743f3-168">Klikněte na tlačítko vpřed.</span><span class="sxs-lookup"><span data-stu-id="743f3-168">Click the forward arrow.</span></span>
5. <span data-ttu-id="743f3-169">Potvrďte mapování adresáře a spolusprávci vliv.</span><span class="sxs-lookup"><span data-stu-id="743f3-169">Confirm the directory mapping and affected co-administrators.</span></span> <span data-ttu-id="743f3-170">Pokud přecházíte z jiného adresáře, budou odebrány všechny spolusprávci z původní adresáře.</span><span class="sxs-lookup"><span data-stu-id="743f3-170">If you are moving from another directory, all the co-administrators from the original directory are removed.</span></span>

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a><span data-ttu-id="743f3-171">Jsem uživatele nebo členem domény na klienta AAD a vytvořili jste předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-171">I'm a domain user/member on the AAD tenant and I've created a preconfigured solution.</span></span> <span data-ttu-id="743f3-172">Jak I přiřazovány roli své aplikaci?</span><span class="sxs-lookup"><span data-stu-id="743f3-172">How do I get assigned a role for my application?</span></span>

<span data-ttu-id="743f3-173">Požádejte o globální správce a zkontrolujte globální správce v tenantovi AAD pak chcete přiřadit role uživatele sami.</span><span class="sxs-lookup"><span data-stu-id="743f3-173">Ask a global administrator to make you a global administrator on the AAD tenant and then assign roles to users yourself.</span></span> <span data-ttu-id="743f3-174">Případně požádejte globální správce, aby vám přiřadil roli přímo.</span><span class="sxs-lookup"><span data-stu-id="743f3-174">Alternatively, ask a global administrator to assign you a role directly.</span></span> <span data-ttu-id="743f3-175">Pokud chcete změnit klienta AAD předkonfigurované řešení byla nasazena do, najdete v části Další otázka.</span><span class="sxs-lookup"><span data-stu-id="743f3-175">If you'd like to change the AAD tenant your preconfigured solution has been deployed to, see the next question.</span></span>

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a><span data-ttu-id="743f3-176">Jak lze přepnout, které Moje předkonfigurovaného řešení vzdáleného monitorování a aplikace jsou přiřazeny k klienta AAD?</span><span class="sxs-lookup"><span data-stu-id="743f3-176">How do I switch the AAD tenant my remote monitoring preconfigured solution and application are assigned to?</span></span>

<span data-ttu-id="743f3-177">Můžete spustit nasazení cloudu z <https://github.com/Azure/azure-iot-remote-monitoring> a znovu nasaďte s nově vytvořený klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-177">You can run a cloud deployment from <https://github.com/Azure/azure-iot-remote-monitoring> and redeploy with a newly created AAD tenant.</span></span> <span data-ttu-id="743f3-178">Protože jsou ve výchozím globálním správcem při vytváření klienta služby AAD máte oprávnění k přidání uživatelů a přiřazování rolí pro tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="743f3-178">Because you are, by default, a global administrator when you create an AAD tenant, you have permissions to add users and assign roles to those users.</span></span>

1. <span data-ttu-id="743f3-179">Vytvořte adresář služby AAD v [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="743f3-179">Create an AAD directory in the [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="743f3-180">Přejděte na <https://github.com/Azure/azure-iot-remote-monitoring>.</span><span class="sxs-lookup"><span data-stu-id="743f3-180">Go to <https://github.com/Azure/azure-iot-remote-monitoring>.</span></span>
3. <span data-ttu-id="743f3-181">Spustit `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (například `build.cmd cloud debug myRMSolution`)</span><span class="sxs-lookup"><span data-stu-id="743f3-181">Run `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (For example, `build.cmd cloud debug myRMSolution`)</span></span>
4. <span data-ttu-id="743f3-182">Po zobrazení výzvy, nastavte **tenantid** být klientovi nově vytvořený místo předchozí klienta.</span><span class="sxs-lookup"><span data-stu-id="743f3-182">When prompted, set the **tenantid** to be your newly created tenant instead of your previous tenant.</span></span>

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a><span data-ttu-id="743f3-183">Změna služby správce nebo Spolusprávce při přihlášení k účtu organizační</span><span class="sxs-lookup"><span data-stu-id="743f3-183">I want to change a Service Administrator or Co-Administrator when logged in with an organisational account</span></span>

<span data-ttu-id="743f3-184">Najdete v článku na podporu [změna správce služeb a Spolusprávce při přihlášení k účtu organizační][lnk-service-admins].</span><span class="sxs-lookup"><span data-stu-id="743f3-184">See the support article [Changing Service Administrator and Co-Administrator when logged in with an organisational account][lnk-service-admins].</span></span>

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a><span data-ttu-id="743f3-185">Proč se zobrazuje tato chyba?</span><span class="sxs-lookup"><span data-stu-id="743f3-185">Why am I seeing this error?</span></span> <span data-ttu-id="743f3-186">"Váš účet nemá dostatečná oprávnění k vytvoření řešení.</span><span class="sxs-lookup"><span data-stu-id="743f3-186">"Your account does not have the proper permissions to create a solution.</span></span> <span data-ttu-id="743f3-187">"Svého účtu správce nebo zkuste to znovu s jiným účtem."</span><span class="sxs-lookup"><span data-stu-id="743f3-187">Please check with your account administrator or try with a different account."</span></span>

<span data-ttu-id="743f3-188">Podívejte se na následující diagram pokyny:</span><span class="sxs-lookup"><span data-stu-id="743f3-188">Look at the following diagram for guidance:</span></span>

![][img-flowchart]

> [!NOTE]
> <span data-ttu-id="743f3-189">Pokud nadále zobrazovat chybu po ověření, které jsou jako globální správce v tenantovi AAD a spolusprávce u předplatného, musí správce účtu odebrat uživatele a přiřadit potřebná oprávnění v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="743f3-189">If you continue to see the error after validating you are a global administrator on the AAD tenant and a co-administrator on the subscription, have your account administrator remove the user and reassign necessary permissions in this order.</span></span> <span data-ttu-id="743f3-190">Nejdřív přidejte uživatele jako globální správce a poté přidejte uživatele jako spolusprávce na předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="743f3-190">First, add the user as a global administrator and then add user as a co-administrator on the Azure subscription.</span></span> <span data-ttu-id="743f3-191">Pokud problémy potrvají, obraťte se na [centru pro nápovědu a podporu][lnk-help-support].</span><span class="sxs-lookup"><span data-stu-id="743f3-191">If issues persist, contact [Help & Support][lnk-help-support].</span></span>

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a><span data-ttu-id="743f3-192">Proč se zobrazuje tato chyba, pokud předplatné Azure?</span><span class="sxs-lookup"><span data-stu-id="743f3-192">Why am I seeing this error when I have an Azure subscription?</span></span> <span data-ttu-id="743f3-193">"K vytvoření předem nakonfigurovaná řešení je potřeba předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="743f3-193">"An Azure subscription is required to create pre-configured solutions.</span></span> <span data-ttu-id="743f3-194">Bezplatný zkušební účet můžete vytvořit během několika minut."</span><span class="sxs-lookup"><span data-stu-id="743f3-194">You can create a free trial account in just a couple of minutes."</span></span>

<span data-ttu-id="743f3-195">Pokud jste si jisti, že máte předplatné Azure, ověření klienta mapování pro vaše předplatné a ujistěte se, že je vybrána správná klienta v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="743f3-195">If you're certain you have an Azure subscription, validate the tenant mapping for your subscription and ensure the correct tenant is selected in the dropdown.</span></span> <span data-ttu-id="743f3-196">Pokud jste ověřit správnost požadované klienta, postupujte podle na předchozím obrázku a ověřit mapování vaše předplatné a tohoto klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="743f3-196">If you’ve validated the desired tenant is correct, follow the preceding diagram and validate the mapping of your subscription and this AAD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="743f3-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="743f3-197">Next steps</span></span>
<span data-ttu-id="743f3-198">Pokračujte ve čtení o IoT Suite, najdete v tématu jak můžete [přizpůsobení předkonfigurovaného řešení][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="743f3-198">To continue learning about IoT Suite, see how you can [customize a preconfigured solution][lnk-customize].</span></span>

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
