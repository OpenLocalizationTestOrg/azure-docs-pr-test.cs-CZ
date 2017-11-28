---
title: aaaAzure IoT Suite a Azure Active Directory | Microsoft Docs
description: "Popisuje, jak Azure IoT Suite využívá Azure Active Directory toomanage oprávnění."
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
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a><span data-ttu-id="dea6d-103">Oprávnění na webu azureiotsuite.com hello</span><span class="sxs-lookup"><span data-stu-id="dea6d-103">Permissions on hello azureiotsuite.com site</span></span>

## <a name="what-happens-when-you-sign-in"></a><span data-ttu-id="dea6d-104">Co se stane, když se přihlásíte</span><span class="sxs-lookup"><span data-stu-id="dea6d-104">What happens when you sign in</span></span>

<span data-ttu-id="dea6d-105">Hello poprvé, přihlaste se na [azureiotsuite.com][lnk-azureiotsuite], lokalita hello Určuje úrovně oprávnění hello založené na hello aktuálně vybrané klienta Azure Active Directory (AAD) a Azure předplatné.</span><span class="sxs-lookup"><span data-stu-id="dea6d-105">hello first time you sign in at [azureiotsuite.com][lnk-azureiotsuite], hello site determines hello permission levels you have based on hello currently selected Azure Active Directory (AAD) tenant and Azure subscription.</span></span>

1. <span data-ttu-id="dea6d-106">Nejprve seznamu toopopulate hello klientům vidět další uživatelského jména tooyour hello lokality zjistí z Azure které AAD klienty patří do.</span><span class="sxs-lookup"><span data-stu-id="dea6d-106">First, toopopulate hello list of tenants seen next tooyour username, hello site finds out from Azure which AAD tenants you belong to.</span></span> <span data-ttu-id="dea6d-107">V současné době hello lokality může obsahovat pouze tokeny uživatele pro jednoho klienta v čase.</span><span class="sxs-lookup"><span data-stu-id="dea6d-107">Currently, hello site can only obtain user tokens for one tenant at a time.</span></span> <span data-ttu-id="dea6d-108">Proto při přepnutí klientů pomocí rozevíracího seznamu hello v pravém horním rohu hello hello lokality přihlásí jste toothat klienta tooobtain hello tokeny pro tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="dea6d-108">Therefore, when you switch tenants using hello dropdown in hello top right corner, hello site logs you in toothat tenant tooobtain hello tokens for that tenant.</span></span>

2. <span data-ttu-id="dea6d-109">V dalším kroku hello lokality zjistí z Azure, které odběry přidružený hello vybrané klienta.</span><span class="sxs-lookup"><span data-stu-id="dea6d-109">Next, hello site finds out from Azure which subscriptions you have associated with hello selected tenant.</span></span> <span data-ttu-id="dea6d-110">Při vytváření nové předkonfigurované řešení zobrazit hello dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="dea6d-110">You see hello available subscriptions when you create a new preconfigured solution.</span></span>

3. <span data-ttu-id="dea6d-111">Nakonec hello lokality načte všechny zdroje hello v hello předplatných a skupin prostředků označené jako předkonfigurovaná řešení a naplní hello dlaždice na domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-111">Finally, hello site retrieves all hello resources in hello subscriptions and resource groups tagged as preconfigured solutions and populates hello tiles on hello home page.</span></span>

<span data-ttu-id="dea6d-112">Hello následující oddíly popisují hello rolí, které řídí přístup toohello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-112">hello following sections describe hello roles that control access toohello preconfigured solutions.</span></span>

## <a name="aad-roles"></a><span data-ttu-id="dea6d-113">Role AAD</span><span class="sxs-lookup"><span data-stu-id="dea6d-113">AAD roles</span></span>

<span data-ttu-id="dea6d-114">role AAD Hello hello možnost zřídit předkonfigurované řešení řídit a spravovat uživatele v předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-114">hello AAD roles control hello ability provision preconfigured solutions and manage users in a preconfigured solution.</span></span>

<span data-ttu-id="dea6d-115">Další informace o rolích správce můžete najít v AAD v [přiřazení rolí správce ve službě Azure AD][lnk-aad-admin].</span><span class="sxs-lookup"><span data-stu-id="dea6d-115">You can find more information about administrator roles in AAD in [Assigning administrator roles in Azure AD][lnk-aad-admin].</span></span> <span data-ttu-id="dea6d-116">Hello aktuální článek zaměřuje na hello **globálního správce** a hello **uživatele** role directory používaného hello předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-116">hello current article focuses on hello **Global Administrator** and hello **User** directory roles as used by hello preconfigured solutions.</span></span>

### <a name="global-administrator"></a><span data-ttu-id="dea6d-117">Globální správce</span><span class="sxs-lookup"><span data-stu-id="dea6d-117">Global administrator</span></span>

<span data-ttu-id="dea6d-118">Může být mnoho globální správci za klienta AAD:</span><span class="sxs-lookup"><span data-stu-id="dea6d-118">There can be many global administrators per AAD tenant:</span></span>

* <span data-ttu-id="dea6d-119">Při vytváření klienta služby AAD jste výchozí hello globální správce tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="dea6d-119">When you create an AAD tenant, you are by default hello global administrator of that tenant.</span></span>
* <span data-ttu-id="dea6d-120">Globální správce Hello můžete zřídit předkonfigurované řešení a je mu přiřazená **správce** role pro aplikaci hello uvnitř jejich klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="dea6d-120">hello global administrator can provision a preconfigured solution and is assigned an **Admin** role for hello application inside their AAD tenant.</span></span>
* <span data-ttu-id="dea6d-121">Pokud jiný uživatel v hello stejné vytvoří klienta AAD aplikace, hello výchozí role uděleno toohello globální správce je **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="dea6d-121">If another user in hello same AAD tenant creates an application, hello default role granted toohello global administrator is **ReadOnly**.</span></span>
* <span data-ttu-id="dea6d-122">Globální správce můžete přiřadit tooroles uživatelů pro aplikace pomocí hello [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="dea6d-122">A global administrator can assign users tooroles for applications using hello [Azure portal][lnk-portal].</span></span>

### <a name="domain-user"></a><span data-ttu-id="dea6d-123">Uživatel domény</span><span class="sxs-lookup"><span data-stu-id="dea6d-123">Domain user</span></span>

<span data-ttu-id="dea6d-124">Může být velký počet uživatelů domény na klienta AAD:</span><span class="sxs-lookup"><span data-stu-id="dea6d-124">There can be many domain users per AAD tenant:</span></span>

* <span data-ttu-id="dea6d-125">Uživatel domény můžete zřídit předkonfigurované řešení prostřednictvím hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality.</span><span class="sxs-lookup"><span data-stu-id="dea6d-125">A domain user can provision a preconfigured solution through hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="dea6d-126">Ve výchozím nastavení, hello domény uživateli je udělen hello **správce** role v hello zřízený aplikace.</span><span class="sxs-lookup"><span data-stu-id="dea6d-126">By default, hello domain user is granted hello **Admin** role in hello provisioned application.</span></span>
* <span data-ttu-id="dea6d-127">Uživatel domény může vytvořit aplikaci pomocí skriptu build.cmd hello v hello [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance] [ lnk-pm-github-repo], nebo [azure-iot připojené factory] [ lnk-cf-github-repo] úložiště.</span><span class="sxs-lookup"><span data-stu-id="dea6d-127">A domain user can create an application using hello build.cmd script in hello [azure-iot-remote-monitoring][lnk-rm-github-repo],  [azure-iot-predictive-maintenance][lnk-pm-github-repo], or [azure-iot-connected-factory][lnk-cf-github-repo] repository.</span></span> <span data-ttu-id="dea6d-128">Ale udělena hello výchozí role uživatele domény toohello je **jen pro čtení**, protože uživatel domény nemá oprávnění tooassign role.</span><span class="sxs-lookup"><span data-stu-id="dea6d-128">However, hello default role granted toohello domain user is **ReadOnly**, because a domain user does not have permission tooassign roles.</span></span>
* <span data-ttu-id="dea6d-129">Pokud jiný uživatel v tenantovi AAD hello vytvoří aplikace, uživatele domény hello je přiřazen hello **jen pro čtení** roli ve výchozím nastavení pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dea6d-129">If another user in hello AAD tenant creates an application, hello domain user is assigned hello **ReadOnly** role by default for that application.</span></span>
* <span data-ttu-id="dea6d-130">Uživatel domény nelze přiřadit role pro aplikace; proto nelze uživatele domény přidat, uživatelé nebo role pro uživatele pro aplikaci i v případě, že se jeho zřízení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-130">A domain user cannot assign roles for applications; therefore a domain user cannot add users or roles for users for an application even if they provisioned it.</span></span>

### <a name="guest-user"></a><span data-ttu-id="dea6d-131">Uživatele Guest</span><span class="sxs-lookup"><span data-stu-id="dea6d-131">Guest User</span></span>

<span data-ttu-id="dea6d-132">Může být velký počet uživatelů typu Host za klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="dea6d-132">There can be many guest users per AAD tenant.</span></span> <span data-ttu-id="dea6d-133">Uživatele typu Host mají omezenou sadu oprávnění v tenantovi AAD hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-133">Guest users have a limited set of rights in hello AAD tenant.</span></span> <span data-ttu-id="dea6d-134">V důsledku toho uživatele typu Host nejde zřídit předkonfigurované řešení v tenantovi AAD hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-134">As a result, guest users cannot provision a preconfigured solution in hello AAD tenant.</span></span>

<span data-ttu-id="dea6d-135">Další informace týkající se uživatelů a rolí v AAD najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="dea6d-135">For more information about users and roles in AAD, see hello following resources:</span></span>

* <span data-ttu-id="dea6d-136">[Vytvořte uživatele ve službě Azure AD][lnk-create-edit-users]</span><span class="sxs-lookup"><span data-stu-id="dea6d-136">[Create users in Azure AD][lnk-create-edit-users]</span></span>
* <span data-ttu-id="dea6d-137">[Přiřazení uživatelů tooapps][lnk-assign-app-roles]</span><span class="sxs-lookup"><span data-stu-id="dea6d-137">[Assign users tooapps][lnk-assign-app-roles]</span></span>

## <a name="azure-subscription-administrator-roles"></a><span data-ttu-id="dea6d-138">Role správce předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="dea6d-138">Azure subscription administrator roles</span></span>

<span data-ttu-id="dea6d-139">role správce Azure Hello řídit hello možnost toomap tooan AD klienta služby předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="dea6d-139">hello Azure admin roles control hello ability toomap an Azure subscription tooan AD tenant.</span></span>

<span data-ttu-id="dea6d-140">Další informace o rolích správce Azure hello v článku hello [jak tooadd nebo změňte Azure Spolusprávcem, Správce služeb a správce účtu][lnk-admin-roles].</span><span class="sxs-lookup"><span data-stu-id="dea6d-140">Find out more about hello Azure admin roles in hello article [How tooadd or change Azure Co-Administrator, Service Administrator, and Account Administrator][lnk-admin-roles].</span></span>

## <a name="application-roles"></a><span data-ttu-id="dea6d-141">Aplikační role</span><span class="sxs-lookup"><span data-stu-id="dea6d-141">Application roles</span></span>

<span data-ttu-id="dea6d-142">Hello aplikační role řízení přístupu toodevices v předkonfigurovaném řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-142">hello application roles control access toodevices in your preconfigured solution.</span></span>

<span data-ttu-id="dea6d-143">Existují dvě definované role a jedna implicitní role, které jsou definované v zřízené aplikaci:</span><span class="sxs-lookup"><span data-stu-id="dea6d-143">There are two defined roles and one implicit role defined in a provisioned application:</span></span>

* <span data-ttu-id="dea6d-144">**Správce:** má úplné řízení tooadd, správu, odeberte zařízení a upravit nastavení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-144">**Admin:** Has full control tooadd, manage, remove devices, and modify settings.</span></span>
* <span data-ttu-id="dea6d-145">**Jen pro čtení:** můžete zobrazit zařízení, pravidla, akce, úlohy a telemetrie.</span><span class="sxs-lookup"><span data-stu-id="dea6d-145">**ReadOnly:** Can view devices, rules, actions, jobs, and telemetry.</span></span>

<span data-ttu-id="dea6d-146">Můžete najít hello oprávnění přiřazenou roli tooeach v hello [RolePermissions.cs] [ lnk-resource-cs] zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="dea6d-146">You can find hello permissions assigned tooeach role in hello [RolePermissions.cs][lnk-resource-cs] source file.</span></span>

### <a name="changing-application-roles-for-a-user"></a><span data-ttu-id="dea6d-147">Změna aplikační role pro uživatele</span><span class="sxs-lookup"><span data-stu-id="dea6d-147">Changing application roles for a user</span></span>

<span data-ttu-id="dea6d-148">Můžete použít následující postup toomake uživatele v Active Directory správcem předkonfigurované řešení hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-148">You can use hello following procedure toomake a user in your Active Directory an administrator of your preconfigured solution.</span></span>

<span data-ttu-id="dea6d-149">Musí být role Globální správce toochange AAD pro uživatele:</span><span class="sxs-lookup"><span data-stu-id="dea6d-149">You must be an AAD global administrator toochange roles for a user:</span></span>

1. <span data-ttu-id="dea6d-150">Přejděte toohello [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="dea6d-150">Go toohello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="dea6d-151">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="dea6d-151">Select **Azure Active Directory**.</span></span>
3. <span data-ttu-id="dea6d-152">Ujistěte se, že používáte hello adresář, který jste zvolili na azureiotsuite.com při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-152">Make sure you are using hello directory you chose on azureiotsuite.com when you provisioned your solution.</span></span> <span data-ttu-id="dea6d-153">Pokud máte několik adresářů, které jsou spojené s vaším předplatným, můžete přepnout mezi nimi Pokud kliknete na název účtu v pravém horním hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="dea6d-153">If you have multiple directories associated with your subscription, you can switch between them if you click your account name at hello top-right of hello portal.</span></span>
4. <span data-ttu-id="dea6d-154">Klikněte na tlačítko **podnikové aplikace, které**, pak **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dea6d-154">Click **Enterprise applications**, then **All applications**.</span></span>
4. <span data-ttu-id="dea6d-155">Zobrazit **všechny aplikace** s **žádné** stavu.</span><span class="sxs-lookup"><span data-stu-id="dea6d-155">Show **All applications** with **Any** status.</span></span> <span data-ttu-id="dea6d-156">Poté vyhledejte aplikaci s názvem předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-156">Then search for an application with name of your preconfigured solution.</span></span>
5. <span data-ttu-id="dea6d-157">Klikněte na název hello hello aplikace, která odpovídá názvu vašeho předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-157">Click hello name of hello application that matches your preconfigured solution name.</span></span>
6. <span data-ttu-id="dea6d-158">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dea6d-158">Click **Users and groups**.</span></span>
7. <span data-ttu-id="dea6d-159">Vyberte uživatele hello chcete tooswitch role.</span><span class="sxs-lookup"><span data-stu-id="dea6d-159">Select hello user you want tooswitch roles.</span></span>
8. <span data-ttu-id="dea6d-160">Klikněte na tlačítko **přiřadit** a vyberte hello role (jako například **správce**) byste chtěli tooassign toohello uživatele, klikněte na tlačítko zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-160">Click **Assign** and select hello role (such as **Admin**) you'd like tooassign toohello user, click hello check mark.</span></span>

## <a name="faq"></a><span data-ttu-id="dea6d-161">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="dea6d-161">FAQ</span></span>

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a><span data-ttu-id="dea6d-162">Správce služby a v nastavit jako toochange hello directory mapování mezi Moje předplatné a konkrétní klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="dea6d-162">I'm a service administrator and I'd like toochange hello directory mapping between my subscription and a specific AAD tenant.</span></span> <span data-ttu-id="dea6d-163">Jak tento úkol provést?</span><span class="sxs-lookup"><span data-stu-id="dea6d-163">How do I complete this task?</span></span>

1. <span data-ttu-id="dea6d-164">Přejděte toohello [portál Azure classic][lnk-classic-portal], klikněte na tlačítko **nastavení** v seznamu služeb na levé straně hello hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-164">Go toohello [Azure classic portal][lnk-classic-portal], click **Settings** in hello list of services on hello left-hand side.</span></span>
2. <span data-ttu-id="dea6d-165">Vyberte hello odběr, který chcete toochange hello directory mapování.</span><span class="sxs-lookup"><span data-stu-id="dea6d-165">Select hello subscription you'd like toochange hello directory mapping to.</span></span>
3. <span data-ttu-id="dea6d-166">Klikněte na tlačítko **upravit adresář**.</span><span class="sxs-lookup"><span data-stu-id="dea6d-166">Click **Edit Directory**.</span></span>
4. <span data-ttu-id="dea6d-167">Vyberte hello **Directory** chcete toouse v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-167">Select hello **Directory** you would like toouse in hello dropdown.</span></span> <span data-ttu-id="dea6d-168">Klikněte na tlačítko Předat dál šipku hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-168">Click hello forward arrow.</span></span>
5. <span data-ttu-id="dea6d-169">Potvrďte mapování hello adresáře a spolusprávci vliv.</span><span class="sxs-lookup"><span data-stu-id="dea6d-169">Confirm hello directory mapping and affected co-administrators.</span></span> <span data-ttu-id="dea6d-170">Pokud přecházíte z jiného adresáře, budou odebrány všechny hello spolusprávci z původní adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-170">If you are moving from another directory, all hello co-administrators from hello original directory are removed.</span></span>

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a><span data-ttu-id="dea6d-171">Jsem uživatele nebo členem domény na hello AAD klienta a vytvořili jste předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-171">I'm a domain user/member on hello AAD tenant and I've created a preconfigured solution.</span></span> <span data-ttu-id="dea6d-172">Jak I přiřazovány roli své aplikaci?</span><span class="sxs-lookup"><span data-stu-id="dea6d-172">How do I get assigned a role for my application?</span></span>

<span data-ttu-id="dea6d-173">Požádejte toomake globálního správce globálního správce na hello AAD klienta a pak mu přiřaďte role toousers sami.</span><span class="sxs-lookup"><span data-stu-id="dea6d-173">Ask a global administrator toomake you a global administrator on hello AAD tenant and then assign roles toousers yourself.</span></span> <span data-ttu-id="dea6d-174">Případně požádejte tooassign globální správce je role přímo.</span><span class="sxs-lookup"><span data-stu-id="dea6d-174">Alternatively, ask a global administrator tooassign you a role directly.</span></span> <span data-ttu-id="dea6d-175">Pokud chcete klienta AAD hello toochange předkonfigurované řešení pro nasazení, najdete v části Další otázka hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-175">If you'd like toochange hello AAD tenant your preconfigured solution has been deployed to, see hello next question.</span></span>

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a><span data-ttu-id="dea6d-176">Jak lze přepnout hello AAD klienta, které Moje předkonfigurovaného řešení vzdáleného monitorování a aplikace jsou přiřazeny k?</span><span class="sxs-lookup"><span data-stu-id="dea6d-176">How do I switch hello AAD tenant my remote monitoring preconfigured solution and application are assigned to?</span></span>

<span data-ttu-id="dea6d-177">Můžete spustit nasazení cloudu z <https://github.com/Azure/azure-iot-remote-monitoring> a znovu nasaďte s nově vytvořený klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="dea6d-177">You can run a cloud deployment from <https://github.com/Azure/azure-iot-remote-monitoring> and redeploy with a newly created AAD tenant.</span></span> <span data-ttu-id="dea6d-178">Protože jsou ve výchozím globálním správcem při vytváření klienta služby AAD máte oprávnění tooadd uživatele a přiřadit role uživatele toothose.</span><span class="sxs-lookup"><span data-stu-id="dea6d-178">Because you are, by default, a global administrator when you create an AAD tenant, you have permissions tooadd users and assign roles toothose users.</span></span>

1. <span data-ttu-id="dea6d-179">Vytvoření adresáře služby AAD v hello [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="dea6d-179">Create an AAD directory in hello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="dea6d-180">Přejděte příliš<https://github.com/Azure/azure-iot-remote-monitoring>.</span><span class="sxs-lookup"><span data-stu-id="dea6d-180">Go too<https://github.com/Azure/azure-iot-remote-monitoring>.</span></span>
3. <span data-ttu-id="dea6d-181">Spustit `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (například `build.cmd cloud debug myRMSolution`)</span><span class="sxs-lookup"><span data-stu-id="dea6d-181">Run `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (For example, `build.cmd cloud debug myRMSolution`)</span></span>
4. <span data-ttu-id="dea6d-182">Po zobrazení výzvy, nastavte hello **tenantid** toobe tenanta nově vytvořený místo předchozího klienta.</span><span class="sxs-lookup"><span data-stu-id="dea6d-182">When prompted, set hello **tenantid** toobe your newly created tenant instead of your previous tenant.</span></span>

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a><span data-ttu-id="dea6d-183">Chci, aby toochange služby správce nebo Spolusprávce při přihlášení k účtu organizační</span><span class="sxs-lookup"><span data-stu-id="dea6d-183">I want toochange a Service Administrator or Co-Administrator when logged in with an organisational account</span></span>

<span data-ttu-id="dea6d-184">Najdete v článku podpory hello [změna správce služeb a Spolusprávce při přihlášení k účtu organizační][lnk-service-admins].</span><span class="sxs-lookup"><span data-stu-id="dea6d-184">See hello support article [Changing Service Administrator and Co-Administrator when logged in with an organisational account][lnk-service-admins].</span></span>

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a><span data-ttu-id="dea6d-185">Proč se zobrazuje tato chyba?</span><span class="sxs-lookup"><span data-stu-id="dea6d-185">Why am I seeing this error?</span></span> <span data-ttu-id="dea6d-186">"Váš účet nemá příslušná oprávnění toocreate hello řešení.</span><span class="sxs-lookup"><span data-stu-id="dea6d-186">"Your account does not have hello proper permissions toocreate a solution.</span></span> <span data-ttu-id="dea6d-187">"Svého účtu správce nebo zkuste to znovu s jiným účtem."</span><span class="sxs-lookup"><span data-stu-id="dea6d-187">Please check with your account administrator or try with a different account."</span></span>

<span data-ttu-id="dea6d-188">Podívejte se na následující diagram pokyny hello:</span><span class="sxs-lookup"><span data-stu-id="dea6d-188">Look at hello following diagram for guidance:</span></span>

![][img-flowchart]

> [!NOTE]
> <span data-ttu-id="dea6d-189">Pokud budete pokračovat toosee hello chybu po ověření jste globální správce v tenantovi AAD hello a spolusprávce předplatného hello, musí správce účtu odebrat hello uživatele a přiřadit potřebná oprávnění v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="dea6d-189">If you continue toosee hello error after validating you are a global administrator on hello AAD tenant and a co-administrator on hello subscription, have your account administrator remove hello user and reassign necessary permissions in this order.</span></span> <span data-ttu-id="dea6d-190">Nejdřív přidejte uživatele hello jako globální správce a poté přidejte uživatele jako spolusprávce na hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="dea6d-190">First, add hello user as a global administrator and then add user as a co-administrator on hello Azure subscription.</span></span> <span data-ttu-id="dea6d-191">Pokud problémy potrvají, obraťte se na [centru pro nápovědu a podporu][lnk-help-support].</span><span class="sxs-lookup"><span data-stu-id="dea6d-191">If issues persist, contact [Help & Support][lnk-help-support].</span></span>

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a><span data-ttu-id="dea6d-192">Proč se zobrazuje tato chyba, pokud předplatné Azure?</span><span class="sxs-lookup"><span data-stu-id="dea6d-192">Why am I seeing this error when I have an Azure subscription?</span></span> <span data-ttu-id="dea6d-193">"Předplatné Azure je řešení vyžaduje toocreate předem nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="dea6d-193">"An Azure subscription is required toocreate pre-configured solutions.</span></span> <span data-ttu-id="dea6d-194">Bezplatný zkušební účet můžete vytvořit během několika minut."</span><span class="sxs-lookup"><span data-stu-id="dea6d-194">You can create a free trial account in just a couple of minutes."</span></span>

<span data-ttu-id="dea6d-195">Pokud jste si jisti, že máte předplatné Azure, ověření klienta hello mapování pro vaše předplatné a ujistěte se, že hello správné klienta je vybraný v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="dea6d-195">If you're certain you have an Azure subscription, validate hello tenant mapping for your subscription and ensure hello correct tenant is selected in hello dropdown.</span></span> <span data-ttu-id="dea6d-196">Pokud jste ověřit hello potřeby správnost klienta, postupujte podle hello předchozím diagramu a ověřit mapování hello vaše předplatné a tohoto klienta AAD.</span><span class="sxs-lookup"><span data-stu-id="dea6d-196">If you’ve validated hello desired tenant is correct, follow hello preceding diagram and validate hello mapping of your subscription and this AAD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dea6d-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dea6d-197">Next steps</span></span>
<span data-ttu-id="dea6d-198">toocontinue získávání informací o IoT Suite, najdete v části jak můžete [přizpůsobení předkonfigurovaného řešení][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="dea6d-198">toocontinue learning about IoT Suite, see how you can [customize a preconfigured solution][lnk-customize].</span></span>

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
