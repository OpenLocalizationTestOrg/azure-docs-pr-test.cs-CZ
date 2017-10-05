---
title: Operace v Azure Active Directory Connect Health
description: "Tento článek popisuje další operace, které lze provést po nasazení Azure AD Connect Health."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 06afc6b4149ea1590a2994d1638d6979a89035e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-connect-health-operations"></a><span data-ttu-id="98d9a-103">Operace v Azure Active Directory Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-103">Azure Active Directory Connect Health operations</span></span>
<span data-ttu-id="98d9a-104">Toto téma popisuje různé operace, které můžete provádět pomocí Azure Active Directory (Azure AD) Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-104">This topic describes the various operations you can perform by using Azure Active Directory (Azure AD) Connect Health.</span></span>

## <a name="enable-email-notifications"></a><span data-ttu-id="98d9a-105">Povolení e-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="98d9a-105">Enable email notifications</span></span>
<span data-ttu-id="98d9a-106">Můžete nakonfigurovat službu Azure AD Connect Health k odesílání e-mailová oznámení, pokud výstrahy indikují, že infrastruktury identity není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="98d9a-106">You can configure the Azure AD Connect Health service to send email notifications when alerts indicate that your identity infrastructure is not healthy.</span></span> <span data-ttu-id="98d9a-107">K tomu dojde, pokud je vygenerována výstraha a pokud je vyřešený.</span><span class="sxs-lookup"><span data-stu-id="98d9a-107">This occurs when an alert is generated, and when it is resolved.</span></span>

![Nastavení oznámení e-mailu snímek Azure AD Connect Health](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> <span data-ttu-id="98d9a-109">E-mailová oznámení jsou ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="98d9a-109">Email notifications are enabled by default.</span></span>
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a><span data-ttu-id="98d9a-110">Chcete-li povolit e-mailová oznámení Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-110">To enable Azure AD Connect Health email notifications</span></span>
1. <span data-ttu-id="98d9a-111">Otevřete **výstrahy** okno pro službu, pro který chcete dostávat e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="98d9a-111">Open the **Alerts** blade for the service for which you want to receive email notification.</span></span>
2. <span data-ttu-id="98d9a-112">Na panelu akcí klikněte na **nastavení oznámení**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-112">From the action bar, click **Notification Settings**.</span></span>
3. <span data-ttu-id="98d9a-113">V e-mailové oznámení přepínači, vyberte **ON**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-113">At the email notification switch, select **ON**.</span></span>
4. <span data-ttu-id="98d9a-114">Zaškrtněte políčko, pokud chcete, aby všichni globální správci pro příjem e-mailová oznámení.</span><span class="sxs-lookup"><span data-stu-id="98d9a-114">Select the check box if you want all global administrators to receive email notifications.</span></span>
5. <span data-ttu-id="98d9a-115">Pokud chcete dostávat e-mailová oznámení na jiné e-mailové adresy, zadejte je do **další příjemců e-mailu** pole.</span><span class="sxs-lookup"><span data-stu-id="98d9a-115">If you want to receive email notifications at any other email addresses, specify them in the **Additional Email Recipients** box.</span></span> <span data-ttu-id="98d9a-116">Chcete-li odebrat e-mailovou adresu z tohoto seznamu, klikněte pravým tlačítkem myši na položku a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-116">To remove an email address from this list, right-click the entry and select **Delete**.</span></span>
6. <span data-ttu-id="98d9a-117">Chcete-li dokončení změn, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-117">To finalize the changes, click **Save**.</span></span> <span data-ttu-id="98d9a-118">Změny se projeví až po uložení.</span><span class="sxs-lookup"><span data-stu-id="98d9a-118">Changes take effect only after you save.</span></span>

## <a name="delete-a-server-or-service-instance"></a><span data-ttu-id="98d9a-119">Odstranění instance serveru nebo služby</span><span class="sxs-lookup"><span data-stu-id="98d9a-119">Delete a server or service instance</span></span>

<span data-ttu-id="98d9a-120">V některých případech můžete chtít odebrání serveru z monitorovány.</span><span class="sxs-lookup"><span data-stu-id="98d9a-120">In some instances, you might want to remove a server from being monitored.</span></span> <span data-ttu-id="98d9a-121">Zde je, co potřebujete vědět o odebrání serveru z služby Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-121">Here's what you need to know to remove a server from the Azure AD Connect Health service.</span></span>

<span data-ttu-id="98d9a-122">Pokud odstraňujete serveru, nezapomeňte z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="98d9a-122">When you're deleting a server, be aware of the following:</span></span>

* <span data-ttu-id="98d9a-123">Tato akce zastaví sběr žádná další data z tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-123">This action stops collecting any further data from that server.</span></span> <span data-ttu-id="98d9a-124">Tento server je odebrán ze služby sledování.</span><span class="sxs-lookup"><span data-stu-id="98d9a-124">This server is removed from the monitoring service.</span></span> <span data-ttu-id="98d9a-125">Po této akci nejste schopni zobrazit nové výstrahy, monitorování nebo využití analytická data pro tento server.</span><span class="sxs-lookup"><span data-stu-id="98d9a-125">After this action, you are not able to view new alerts, monitoring, or usage analytics data for this server.</span></span>
* <span data-ttu-id="98d9a-126">Tato akce neodinstaluje agenta stavu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-126">This action does not uninstall the Health Agent from your server.</span></span> <span data-ttu-id="98d9a-127">Pokud jste agenta stavu neodinstalovali před provedením tohoto kroku, může se zobrazit chyby související s agenta stavu na serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-127">If you have not uninstalled the Health Agent before performing this step, you might see errors related to the Health Agent on the server.</span></span>
* <span data-ttu-id="98d9a-128">Tato akce neodstraní data již shromážděná z tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-128">This action does not delete the data already collected from this server.</span></span> <span data-ttu-id="98d9a-129">Data odstraněna podle zásad uchovávání dat Azure.</span><span class="sxs-lookup"><span data-stu-id="98d9a-129">That data is deleted in accordance with the Azure data retention policy.</span></span>
* <span data-ttu-id="98d9a-130">Po provedení této akce, pokud chcete začít monitorovat stejný server znovu, musíte odinstalovat a znovu nainstalujte agenta stavu na tomto serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-130">After performing this action, if you want to start monitoring the same server again, you must uninstall and reinstall the Health Agent on this server.</span></span>

### <a name="to-delete-a-server-from-the-azure-ad-connect-health-service"></a><span data-ttu-id="98d9a-131">Odstranění serveru ze služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-131">To delete a server from the Azure AD Connect Health service</span></span>
<span data-ttu-id="98d9a-132">Azure AD Connect Health pro Active Directory Federation Services (AD FS) a Azure AD Connect (Sync):</span><span class="sxs-lookup"><span data-stu-id="98d9a-132">Azure AD Connect Health for Active Directory Federation Services (AD FS) and Azure AD Connect (Sync):</span></span>

1. <span data-ttu-id="98d9a-133">Otevřete **Server** okno z **seznam serverů** tak, že vyberete serveru název odeberou.</span><span class="sxs-lookup"><span data-stu-id="98d9a-133">Open the **Server** blade from the **Server List** blade by selecting the server name to be removed.</span></span>
2. <span data-ttu-id="98d9a-134">Na **Server** , z panelu akcí klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-134">On the **Server** blade, from the action bar, click **Delete**.</span></span>
3. <span data-ttu-id="98d9a-135">Potvrďte zadáním názvu serveru v poli potvrzení.</span><span class="sxs-lookup"><span data-stu-id="98d9a-135">Confirm by typing the server name in the confirmation box.</span></span>
4. <span data-ttu-id="98d9a-136">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-136">Click **Delete**.</span></span>

<span data-ttu-id="98d9a-137">Azure AD Connect Health pro službu Azure Active Directory Domain Services:</span><span class="sxs-lookup"><span data-stu-id="98d9a-137">Azure AD Connect Health for Azure Active Directory Domain Services:</span></span>

1. <span data-ttu-id="98d9a-138">Otevřete **řadiče domény** řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="98d9a-138">Open the **Domain Controllers** dashboard.</span></span>
2. <span data-ttu-id="98d9a-139">Vyberte řadič domény odeberou.</span><span class="sxs-lookup"><span data-stu-id="98d9a-139">Select the domain controller to be removed.</span></span>
3. <span data-ttu-id="98d9a-140">Na panelu akcí klikněte na **odstranit vybrané**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-140">From the action bar, click **Delete Selected**.</span></span>
4. <span data-ttu-id="98d9a-141">Potvrzení této akce se odstranit server.</span><span class="sxs-lookup"><span data-stu-id="98d9a-141">Confirm the action to delete the server.</span></span>
5. <span data-ttu-id="98d9a-142">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-142">Click **Delete**.</span></span>

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a><span data-ttu-id="98d9a-143">Odstranění instance služby ze služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-143">Delete a service instance from Azure AD Connect Health service</span></span>
<span data-ttu-id="98d9a-144">V některých případech můžete chtít odebrat instanci služby.</span><span class="sxs-lookup"><span data-stu-id="98d9a-144">In some instances, you might want to remove a service instance.</span></span> <span data-ttu-id="98d9a-145">Zde je, co potřebujete vědět, abyste mohli odebrat instanci služby ze služby Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-145">Here's what you need to know to remove a service instance from the Azure AD Connect Health service.</span></span>

<span data-ttu-id="98d9a-146">Pokud odstraňujete instance služby, uvědomte si z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="98d9a-146">When you're deleting a service instance, be aware of the following:</span></span>

* <span data-ttu-id="98d9a-147">Tato akce odebere aktuální instanci služby ze služby sledování.</span><span class="sxs-lookup"><span data-stu-id="98d9a-147">This action removes the current service instance from the monitoring service.</span></span>
* <span data-ttu-id="98d9a-148">Tato akce neodinstaluje ani neodebere agenta stavu ze všech serverů, které byly monitorovány v rámci této instance služby.</span><span class="sxs-lookup"><span data-stu-id="98d9a-148">This action does not uninstall or remove the Health Agent from any of the servers that were monitored as part of this service instance.</span></span> <span data-ttu-id="98d9a-149">Pokud jste agenta stavu neodinstalovali před provedením tohoto kroku, může se zobrazit chyby související s agenta stavu na serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-149">If you have not uninstalled the Health Agent before performing this step, you might see errors related to the Health Agent on the servers.</span></span>
* <span data-ttu-id="98d9a-150">Všechna data z této instance služby se odstraní podle zásad uchovávání dat Azure.</span><span class="sxs-lookup"><span data-stu-id="98d9a-150">All data from this service instance is deleted in accordance with the Azure data retention policy.</span></span>
* <span data-ttu-id="98d9a-151">Po provedení této akce, pokud chcete začít sledovat tuto službu, odinstalujte a znovu nainstalujte agenta stavu ve všech serverech.</span><span class="sxs-lookup"><span data-stu-id="98d9a-151">After performing this action, if you want to start monitoring the service, uninstall and reinstall the Health Agent on all the servers.</span></span> <span data-ttu-id="98d9a-152">Po provedení této akce, pokud chcete začít monitorovat stejný server znovu, odinstalovat, přeinstalovat a registrace agenta stavu na tomto serveru.</span><span class="sxs-lookup"><span data-stu-id="98d9a-152">After performing this action, if you want to start monitoring the same server again, uninstall, reinstall, and register the Health Agent on that server.</span></span>

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a><span data-ttu-id="98d9a-153">Chcete-li odstranit instanci služby ze služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-153">To delete a service instance from the Azure AD Connect Health service</span></span>
1. <span data-ttu-id="98d9a-154">Otevřete **služby** okno z **seznam služeb** okno výběrem identifikátor služby (název farmy), který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="98d9a-154">Open the **Service** blade from the **Service List** blade by selecting the service identifier (farm name) that you want to remove.</span></span>
2. <span data-ttu-id="98d9a-155">Na **Server** , z panelu akcí klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-155">On the **Server** blade, from the action bar, click **Delete**.</span></span>
3. <span data-ttu-id="98d9a-156">Potvrďte zadáním názvu služby do pole pro potvrzení (například:. sts.contoso.com).</span><span class="sxs-lookup"><span data-stu-id="98d9a-156">Confirm by typing the service name in the confirmation box (for example: sts.contoso.com).</span></span>
4. <span data-ttu-id="98d9a-157">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-157">Click **Delete**.</span></span>
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a><span data-ttu-id="98d9a-158">Správa přístupu pomocí řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="98d9a-158">Manage access with Role-Based Access Control</span></span>
<span data-ttu-id="98d9a-159">[Řízení přístupu na základě rolí (RBAC)](../role-based-access-control-configure.md) pro Azure AD Connect Health poskytuje přístup uživatelům a skupinám než globální správci.</span><span class="sxs-lookup"><span data-stu-id="98d9a-159">[Role-Based Access Control (RBAC)](../role-based-access-control-configure.md) for Azure AD Connect Health provides access to users and groups other than global administrators.</span></span> <span data-ttu-id="98d9a-160">RBAC přiřadí role pro příslušné uživatele a skupiny a poskytuje mechanismus pro omezení globálních správců v adresáři.</span><span class="sxs-lookup"><span data-stu-id="98d9a-160">RBAC assigns roles to the intended users and groups, and provides a mechanism to limit the global administrators within your directory.</span></span>

### <a name="roles"></a><span data-ttu-id="98d9a-161">Role</span><span class="sxs-lookup"><span data-stu-id="98d9a-161">Roles</span></span>
<span data-ttu-id="98d9a-162">Azure AD Connect Health podporuje následující předdefinované role:</span><span class="sxs-lookup"><span data-stu-id="98d9a-162">Azure AD Connect Health supports the following built-in roles:</span></span>

| <span data-ttu-id="98d9a-163">Role</span><span class="sxs-lookup"><span data-stu-id="98d9a-163">Role</span></span> | <span data-ttu-id="98d9a-164">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="98d9a-164">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="98d9a-165">Vlastník</span><span class="sxs-lookup"><span data-stu-id="98d9a-165">Owner</span></span> |<span data-ttu-id="98d9a-166">Mohou vlastníci *spravovat přístup* (například přiřazení role uživatele nebo skupiny), *prohlížet všechny informace* (například zobrazení výstrahy) z portálu, a *změnit nastavení* (pro Příklad, e-mailových oznámení) v rámci Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-166">Owners can *manage access* (for example, assign a role to a user or group), *view all information* (for example, view alerts) from the portal, and *change settings* (for example, email notifications) within Azure AD Connect Health.</span></span> <br><span data-ttu-id="98d9a-167">Ve výchozím nastavení tuto roli nemají přiřazenou globální správce Azure AD a toto nastavení nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="98d9a-167">By default, Azure AD global administrators are assigned this role, and this cannot be changed.</span></span> |
| <span data-ttu-id="98d9a-168">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="98d9a-168">Contributor</span></span> |<span data-ttu-id="98d9a-169">Přispěvatelé můžou *prohlížet všechny informace* (například zobrazení výstrahy) z portálu, a *změnit nastavení* (například e-mailová oznámení) v rámci Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-169">Contributors can *view all information* (for example, view alerts) from the portal, and *change settings* (for example, email notifications) within Azure AD Connect Health.</span></span> |
| <span data-ttu-id="98d9a-170">Čtenář</span><span class="sxs-lookup"><span data-stu-id="98d9a-170">Reader</span></span> |<span data-ttu-id="98d9a-171">Můžete čtečky *prohlížet všechny informace* (například zobrazení výstrahy) z portálu v Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-171">Readers can *view all information* (for example, view alerts) from the portal within Azure AD Connect Health.</span></span> |

<span data-ttu-id="98d9a-172">Všechny ostatní role (například uživatel přístup správci nebo uživatelé DevTest Labs) nemají žádný vliv na přístup v rámci Azure AD Connect Health, i když tyto role jsou k dispozici v prostředí portálu.</span><span class="sxs-lookup"><span data-stu-id="98d9a-172">All other roles (such as User Access Administrators or DevTest Labs Users) have no impact to access within Azure AD Connect Health, even if the roles are available in the portal experience.</span></span>

### <a name="access-scope"></a><span data-ttu-id="98d9a-173">Obor přístupu</span><span class="sxs-lookup"><span data-stu-id="98d9a-173">Access scope</span></span>
<span data-ttu-id="98d9a-174">Azure AD Connect Health podporuje správu přístupu na dvou úrovních:</span><span class="sxs-lookup"><span data-stu-id="98d9a-174">Azure AD Connect Health supports managing access at two levels:</span></span>

* <span data-ttu-id="98d9a-175">**Všechny instance služby**: Jedná se o doporučený ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="98d9a-175">**All service instances**: This is the recommended path in most cases.</span></span> <span data-ttu-id="98d9a-176">Určuje přístup pro všechny instance služby (například farmu služby AD FS), typů role, které jsou monitorovány pomocí Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-176">It controls access for all service instances (for example, an AD FS farm) across all role types that are being monitored by Azure AD Connect Health.</span></span>
* <span data-ttu-id="98d9a-177">**Instance služby**: V některých případech možná budete muset oddělit přístupu založených na typech role nebo instance služby.</span><span class="sxs-lookup"><span data-stu-id="98d9a-177">**Service instance**: In some cases, you might need to segregate access based on role types or by a service instance.</span></span> <span data-ttu-id="98d9a-178">V takovém případě můžete spravovat přístup na úrovni instance služby.</span><span class="sxs-lookup"><span data-stu-id="98d9a-178">In this case, you can manage access at the service instance level.</span></span>  

<span data-ttu-id="98d9a-179">Je povoleno, pokud koncový uživatel má přístup buď na directory nebo služby na úrovni instance.</span><span class="sxs-lookup"><span data-stu-id="98d9a-179">Permission is granted if an end user has access either at the directory or service instance level.</span></span>

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a><span data-ttu-id="98d9a-180">Povolit uživatelů nebo skupin přístup k Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-180">Allow users or groups access to Azure AD Connect Health</span></span>
<span data-ttu-id="98d9a-181">Následující kroky ukazují, jak povolit přístup.</span><span class="sxs-lookup"><span data-stu-id="98d9a-181">The following steps show how to allow access.</span></span>
#### <a name="step-1-select-the-appropriate-access-scope"></a><span data-ttu-id="98d9a-182">Krok 1: Vyberte odpovídající přístup obor</span><span class="sxs-lookup"><span data-stu-id="98d9a-182">Step 1: Select the appropriate access scope</span></span>
<span data-ttu-id="98d9a-183">Povolit přístup uživatele na *všech instancí služby* úrovně v rámci Azure AD Connect Health, otevřete okno hlavní v Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-183">To allow a user access at the *all service instances* level within Azure AD Connect Health, open the main blade in Azure AD Connect Health.</span></span><br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a><span data-ttu-id="98d9a-184">Krok 2: Přidání uživatelů a skupin a přiřadit role</span><span class="sxs-lookup"><span data-stu-id="98d9a-184">Step 2: Add users and groups, and assign roles</span></span>
1. <span data-ttu-id="98d9a-185">Z **konfigurace** klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-185">From the **Configure** section, click **Users**.</span></span><br><span data-ttu-id="98d9a-186">
   ![Snímek obrazovky Azure AD připojit RBAC stavu hlavním okně, s uživateli zvýrazněná](./media/active-directory-aadconnect-health/RBAC_main_blade.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-186">
![Screenshot of Azure AD Connect Health RBAC main blade, with Users highlighted](./media/active-directory-aadconnect-health/RBAC_main_blade.png)</span></span>
2. <span data-ttu-id="98d9a-187">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-187">Select **Add**.</span></span>
3. <span data-ttu-id="98d9a-188">V **vyberte roli** podokně, vyberte roli (například **vlastníka**).</span><span class="sxs-lookup"><span data-stu-id="98d9a-188">In the **Select a role** pane, select a role (for example, **Owner**).</span></span><br><span data-ttu-id="98d9a-189">
   ![Snímek Azure AD Connect Health RBAC uživatelé okno](./media/active-directory-aadconnect-health/RBAC_add.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-189">
![Screenshot of Azure AD Connect Health RBAC Users window](./media/active-directory-aadconnect-health/RBAC_add.png)</span></span>
4. <span data-ttu-id="98d9a-190">Zadejte název nebo identifikátor cílové uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="98d9a-190">Type the name or identifier of the targeted user or group.</span></span> <span data-ttu-id="98d9a-191">Současně můžete vybrat jeden nebo více uživatelů nebo skupin.</span><span class="sxs-lookup"><span data-stu-id="98d9a-191">You can select one or more users or groups at the same time.</span></span> <span data-ttu-id="98d9a-192">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-192">Click **Select**.</span></span>
   <span data-ttu-id="98d9a-193">![Snímek Azure AD Connect Health RBAC uživatelé okno](./media/active-directory-aadconnect-health/RBAC_select_users.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-193">![Screenshot of Azure AD Connect Health RBAC Users window](./media/active-directory-aadconnect-health/RBAC_select_users.png)</span></span>
5. <span data-ttu-id="98d9a-194">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-194">Select **OK**.</span></span><br>
6. <span data-ttu-id="98d9a-195">Po dokončení přiřazení role uživatele a skupiny v seznamu se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="98d9a-195">After the role assignment is complete, the users and groups appear in the list.</span></span><br><span data-ttu-id="98d9a-196">
   ![Připojení stavu RBAC uživatelům okno snímek obrazovky Azure AD, s nových uživatelů zvýrazněných](./media/active-directory-aadconnect-health/RBAC_user_list.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-196">
![Screenshot of Azure AD Connect Health RBAC Users window, with new users highlighted](./media/active-directory-aadconnect-health/RBAC_user_list.png)</span></span>

<span data-ttu-id="98d9a-197">Nyní uvedené uživatele a skupiny mají přístup, podle jejich přiřazené role.</span><span class="sxs-lookup"><span data-stu-id="98d9a-197">Now the listed users and groups have access, according to their assigned roles.</span></span>

> [!NOTE]
> * <span data-ttu-id="98d9a-198">Globální správci vždy mají plný přístup ke všem operacím, ale nejsou k dispozici v předchozím seznamu účtů globální správce.</span><span class="sxs-lookup"><span data-stu-id="98d9a-198">Global administrators always have full access to all the operations, but global administrator accounts are not present in the preceding list.</span></span>
> * <span data-ttu-id="98d9a-199">Funkce pozvat uživatele není podporována v rámci Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="98d9a-199">The Invite Users feature is not supported within Azure AD Connect Health.</span></span>
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a><span data-ttu-id="98d9a-200">Krok 3: Sdílejte okno umístění k uživatelům nebo skupinám</span><span class="sxs-lookup"><span data-stu-id="98d9a-200">Step 3: Share the blade location with users or groups</span></span>
1. <span data-ttu-id="98d9a-201">Po přiřazení oprávnění, může uživatel získat přístup Azure AD Connect Health přechodem [zde](http://aka.ms/aadconnecthealth).</span><span class="sxs-lookup"><span data-stu-id="98d9a-201">After you assign permissions, a user can access Azure AD Connect Health by going [here](http://aka.ms/aadconnecthealth).</span></span>
2. <span data-ttu-id="98d9a-202">V okně uživatele můžete Připnout okno nebo různé části, na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="98d9a-202">On the blade, the user can pin the blade, or different parts of it, to the dashboard.</span></span> <span data-ttu-id="98d9a-203">Jednoduše klikněte **připnout na řídicí panel** ikonu.</span><span class="sxs-lookup"><span data-stu-id="98d9a-203">Simply click the **Pin to dashboard** icon.</span></span><br><span data-ttu-id="98d9a-204">
   ![Snímek Azure AD Connect Health RBAC Připnout okno, se zvýrazněnou ikonu Připnutí](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-204">
![Screenshot of Azure AD Connect Health RBAC pin blade, with pin icon highlighted](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)</span></span>

> [!NOTE]
> <span data-ttu-id="98d9a-205">Uživatel s přiřazenou roli čtečky není možné získat rozšíření Azure AD Connect Health z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="98d9a-205">A user with the Reader role assigned is not able to get Azure AD Connect Health extension from the Azure Marketplace.</span></span> <span data-ttu-id="98d9a-206">Uživatele nelze provést nezbytné operace Uděláte to tak "vytvoření".</span><span class="sxs-lookup"><span data-stu-id="98d9a-206">The user cannot perform the necessary "create" operation to do so.</span></span> <span data-ttu-id="98d9a-207">Uživatele můžete pořád dostat do okna přechodem předchozí odkaz na.</span><span class="sxs-lookup"><span data-stu-id="98d9a-207">The user can still get to the blade by going to the preceding link.</span></span> <span data-ttu-id="98d9a-208">Pro následné použití můžete uživatele Připnout okno na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="98d9a-208">For subsequent usage, the user can pin the blade to the dashboard.</span></span>
>
>

### <a name="remove-users-or-groups"></a><span data-ttu-id="98d9a-209">Odebrat uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="98d9a-209">Remove users or groups</span></span>
<span data-ttu-id="98d9a-210">Můžete odebrat uživatele nebo skupinu přidat do Azure AD Connect Health RBAC.</span><span class="sxs-lookup"><span data-stu-id="98d9a-210">You can remove a user or a group added to Azure AD Connect Health RBAC.</span></span> <span data-ttu-id="98d9a-211">Jednoduše klikněte pravým tlačítkem na uživatele nebo skupinu a vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="98d9a-211">Simply right-click the user or group, and select **Remove**.</span></span><br><span data-ttu-id="98d9a-212">
![Připojení stavu RBAC uživatelům okno snímek obrazovky Azure AD, s odebrat zvýrazněná](./media/active-directory-aadconnect-health/RBAC_remove.png)</span><span class="sxs-lookup"><span data-stu-id="98d9a-212">
![Screenshot of Azure AD Connect Health RBAC Users window, with Remove highlighted](./media/active-directory-aadconnect-health/RBAC_remove.png)</span></span>

[//]: # (End of RBAC section)

## <a name="next-steps"></a><span data-ttu-id="98d9a-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98d9a-213">Next steps</span></span>
* [<span data-ttu-id="98d9a-214">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-214">Azure AD Connect Health</span></span>](active-directory-aadconnect-health.md)
* [<span data-ttu-id="98d9a-215">Instalace agenta služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-215">Azure AD Connect Health Agent installation</span></span>](active-directory-aadconnect-health-agent-install.md)
* [<span data-ttu-id="98d9a-216">Používání služby Azure AD Connect Health se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="98d9a-216">Using Azure AD Connect Health with AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
* [<span data-ttu-id="98d9a-217">Používání služby Azure AD Connect Health pro synchronizaci</span><span class="sxs-lookup"><span data-stu-id="98d9a-217">Using Azure AD Connect Health for sync</span></span>](active-directory-aadconnect-health-sync.md)
* [<span data-ttu-id="98d9a-218">Používání služby Azure AD Connect Health se službou AD DS</span><span class="sxs-lookup"><span data-stu-id="98d9a-218">Using Azure AD Connect Health with AD DS</span></span>](active-directory-aadconnect-health-adds.md)
* [<span data-ttu-id="98d9a-219">Azure AD Connect Health – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="98d9a-219">Azure AD Connect Health FAQ</span></span>](active-directory-aadconnect-health-faq.md)
* [<span data-ttu-id="98d9a-220">Historie verzí služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="98d9a-220">Azure AD Connect Health version history</span></span>](active-directory-aadconnect-health-version-history.md)
