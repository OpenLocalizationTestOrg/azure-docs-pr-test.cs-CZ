---
title: Jak aktivovat nebo deaktivovat roli | Microsoft Docs
description: "Informace o aktivaci role pro privilegované identity s aplikací Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: a143fd78eae52fda0cbadb7e74fd8209f24629a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="33147-103">Postup aktivace nebo deaktivace role v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="33147-103">How to activate or deactivate roles in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="33147-104">Azure Active Directory (AD) Privileged Identity Management zjednodušuje, jak spravovat podniky privilegovaný přístup k prostředkům v Azure AD a dalším službám Microsoft online jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="33147-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="33147-105">Pokud jste zadali byl způsobilý pro roli správce, která znamená, že tato role může aktivovat, pokud budete potřebovat k provedení privilegovaných akcí.</span><span class="sxs-lookup"><span data-stu-id="33147-105">If you have been made eligible for an administrative role, that means you can activate that role when you need to perform privileged actions.</span></span> <span data-ttu-id="33147-106">Například pokud někdy spravujete funkcí Office 365, vaší organizace privilegované role správců nemusí mít je trvalé globální správce, vzhledem k tomu, že tato role má to dopad na jiné služby příliš.</span><span class="sxs-lookup"><span data-stu-id="33147-106">For example, if you occasionally manage Office 365 features, your organization's privileged role administrators may not make you a permanent Global Administrator, since that role impacts other services, too.</span></span> <span data-ttu-id="33147-107">Místo toho budou požadovat, abyste vhodné pro Azure AD rolí, například správce serveru Exchange Online.</span><span class="sxs-lookup"><span data-stu-id="33147-107">Instead, they make you eligible for Azure AD roles such as Exchange Online Administrator.</span></span> <span data-ttu-id="33147-108">Může požádat o k aktivaci této role, když potřebujete svá oprávnění a potom budete mít kontroly správce po předem určenou dobu.</span><span class="sxs-lookup"><span data-stu-id="33147-108">You can request to activate that role when you need its privileges, and then you'll have admin control for a predetermined time period.</span></span>

<span data-ttu-id="33147-109">Tento článek je pro správce, který je nutné aktivovat jejich role v Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="33147-109">This article is for admins who need to activate their role in Azure AD Privileged Identity Management (PIM).</span></span> <span data-ttu-id="33147-110">Provede vás provede kroky pro aktivaci role, když potřebujete oprávnění a deaktivovat roli, když jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="33147-110">It walks you through the steps to activate a role when you need the permissions, and deactivate the role when you're done.</span></span> <span data-ttu-id="33147-111">Kromě toho privilegované role správců může vyžadovat schválení pro aktivaci role (Preview).</span><span class="sxs-lookup"><span data-stu-id="33147-111">In addition, privileged role administrators can require approval to activate a role (Preview).</span></span> <span data-ttu-id="33147-112">Další informace o [PIM schválení pracovních](./privileged-identity-management/azure-ad-pim-approval-workflow.md) sem.</span><span class="sxs-lookup"><span data-stu-id="33147-112">Learn more about [PIM Approval Workflows](./privileged-identity-management/azure-ad-pim-approval-workflow.md) here.</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="33147-113">Přidání aplikace Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="33147-113">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="33147-114">Pomocí aplikace Azure AD Privileged Identity Management [portál Azure](https://portal.azure.com/) požádat o aktivaci role, i v případě, že se chystáte provozovat v jiném portálu nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33147-114">Use the Azure AD Privileged Identity Management application in the [Azure portal](https://portal.azure.com/) to request a role activation, even if you're going to operate in another portal or PowerShell.</span></span> <span data-ttu-id="33147-115">Pokud nemáte aplikaci Azure AD Privileged Identity Management na portálu Azure, postupujte podle těchto kroků, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="33147-115">If you don't have the Azure AD Privileged Identity Management application on your Azure portal, follow these steps to get started.</span></span>

1. <span data-ttu-id="33147-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="33147-116">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="33147-117">Vyberte své uživatelské jméno v pravém horním rohu portálu Azure a vyberte adresář, kde vám budou pracovat.</span><span class="sxs-lookup"><span data-stu-id="33147-117">Select your username in the upper right-hand corner of the Azure portal, and select the directory where you will you be operating.</span></span>
3. <span data-ttu-id="33147-118">Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="33147-118">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="33147-119">Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="33147-119">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="33147-120">Aplikace Privileged Identity Management se otevře.</span><span class="sxs-lookup"><span data-stu-id="33147-120">The Privileged Identity Management application opens.</span></span>

## <a name="activate-a-role"></a><span data-ttu-id="33147-121">Aktivovat roli</span><span class="sxs-lookup"><span data-stu-id="33147-121">Activate a role</span></span>
<span data-ttu-id="33147-122">Pokud je nutné vzít v roli, můžete požádat o aktivaci výběrem **Moje role** levé sloupec navigační prvku možnost navigace v aplikaci Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="33147-122">When you need to take on a role, you can request activation by selecting the **My Roles** navigation option in the Azure AD Privileged Identity Management application's left navigation column.</span></span>

1. <span data-ttu-id="33147-123">Přihlaste se k [portál Azure](https://portal.azure.com/) a vyberte dlaždici Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="33147-123">Sign in to the [Azure portal](https://portal.azure.com/) and select the Azure AD Privileged Identity Management tile.</span></span>
2. <span data-ttu-id="33147-124">Vyberte **Moje role**.</span><span class="sxs-lookup"><span data-stu-id="33147-124">Select **My Roles**.</span></span> <span data-ttu-id="33147-125">V seskupení v horní části stránky se zobrazí seznam přiřazené vhodné role.</span><span class="sxs-lookup"><span data-stu-id="33147-125">A list of your assigned eligible roles appear in the grouping at the top of the page.</span></span>
3. <span data-ttu-id="33147-126">Vyberte roli, kterou chcete aktivovat.</span><span class="sxs-lookup"><span data-stu-id="33147-126">Select a role to activate.</span></span>
4. <span data-ttu-id="33147-127">Vyberte **aktivovat**.</span><span class="sxs-lookup"><span data-stu-id="33147-127">Select **Activate**.</span></span> <span data-ttu-id="33147-128">**Žádosti o aktivaci role** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="33147-128">The **Request role activation** blade appears.</span></span>
5. <span data-ttu-id="33147-129">Některé role vyžadují služby Multi-Factor Authentication (MFA) před aktivací roli.</span><span class="sxs-lookup"><span data-stu-id="33147-129">Some roles require Multi-Factor Authentication (MFA) before you can activate the role.</span></span> <span data-ttu-id="33147-130">Můžete mít pouze k ověření na začátku každé relace.</span><span class="sxs-lookup"><span data-stu-id="33147-130">You only have to authenticate once per session.</span></span>
   
    ![Ověřit pomocí vícefaktorového ověřování předtím, než aktivace role – snímek obrazovky][2]
6. <span data-ttu-id="33147-132">V textovém poli zadejte důvod pro žádost o aktivaci.</span><span class="sxs-lookup"><span data-stu-id="33147-132">Enter the reason for the activation request in the text field.</span></span>  <span data-ttu-id="33147-133">Některé role vyžadují, abyste zadat číslo lístku řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="33147-133">Some roles require you to supply a trouble ticket number.</span></span>
7. <span data-ttu-id="33147-134">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="33147-134">Select **OK**.</span></span>  <span data-ttu-id="33147-135">Pokud roli nevyžaduje schválení, bude aktivován a role se zobrazí v seznamu aktivní role (přímo pod seznamem přiřazení role vhodné).</span><span class="sxs-lookup"><span data-stu-id="33147-135">If the role does not require approval, it is now activated, and the role appears in the list of active roles (directly below the list of eligible role assignments).</span></span> <span data-ttu-id="33147-136">Pokud [role vyžaduje schválení](./privileged-identity-management/azure-ad-pim-approval-workflow.md) Pokud chcete aktivovat, se krátce zobrazí oznámení s informační zprávou v pravém horním rohu prohlížeče vás informuje o požadavek čeká na schválení.</span><span class="sxs-lookup"><span data-stu-id="33147-136">If the [role requires approval](./privileged-identity-management/azure-ad-pim-approval-workflow.md) to activate, a toast notification will briefly appear in the upper right-hand corner of your browser informing you the request is pending approval.</span></span>

    ![Žádost čeká na oznámení – snímek obrazovky][3]

## <a name="deactivate-a-role"></a><span data-ttu-id="33147-138">Deaktivovat roli</span><span class="sxs-lookup"><span data-stu-id="33147-138">Deactivate a role</span></span>
<span data-ttu-id="33147-139">Po aktivaci role automaticky deaktivuje po dosažení jeho časový limit (vhodné doba trvání).</span><span class="sxs-lookup"><span data-stu-id="33147-139">Once a role has been activated, it automatically deactivates when its time limit (eligible duration) is reached.</span></span>

<span data-ttu-id="33147-140">Pokud již v rané fázi dokončení úlohy správy, můžete také deaktivovat roli ručně v aplikaci Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="33147-140">If you complete your admin tasks early, you can also deactivate a role manually in the Azure AD Privileged Identity Management application.</span></span>  <span data-ttu-id="33147-141">Vyberte **Moje role**, zvolte roli s tím budete hotovi pomocí z **přiřazení Active role** seskupování a vyberte možnost **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="33147-141">Select **My Roles**, choose the role you're done using from the **Active role assignments** grouping, and select **Deactivate**.</span></span>  

## <a name="cancel-a-pending-request"></a><span data-ttu-id="33147-142">Zrušení čekající žádosti o</span><span class="sxs-lookup"><span data-stu-id="33147-142">Cancel a pending request</span></span>
<span data-ttu-id="33147-143">V případě, že nechcete, aby aktivace role, který vyžaduje schválení, můžete kdykoli zrušit čekající žádosti.</span><span class="sxs-lookup"><span data-stu-id="33147-143">In the event you do not require activation of a role that requires approval, you may cancel a pending request at any time.</span></span> <span data-ttu-id="33147-144">Jednoduše vyberte **Moje role** levé sloupec navigační prvku možnost navigace v aplikaci Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="33147-144">Simply select the **My Roles** navigation option in the Azure AD Privileged Identity Management application's left navigation column.</span></span>

1. <span data-ttu-id="33147-145">Přihlaste se k [portál Azure](https://portal.azure.com/) a vyberte dlaždici Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="33147-145">Sign in to the [Azure portal](https://portal.azure.com/) and select the Azure AD Privileged Identity Management tile.</span></span>
2. <span data-ttu-id="33147-146">Vyberte **Moje role**.</span><span class="sxs-lookup"><span data-stu-id="33147-146">Select **My Roles**.</span></span> <span data-ttu-id="33147-147">V seskupení v horní části stránky se zobrazí seznam přiřazené vhodné role.</span><span class="sxs-lookup"><span data-stu-id="33147-147">A list of your assigned eligible roles appear in the grouping at the top of the page.</span></span>
3. <span data-ttu-id="33147-148">Vyberte roli.</span><span class="sxs-lookup"><span data-stu-id="33147-148">Select a role.</span></span>
4. <span data-ttu-id="33147-149">Vyberte **Aktivace čeká na schválení** hlavičku v okně Podrobnosti o aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="33147-149">Select the **Activation is pending approval** banner on the role activation details blade.</span></span>
5. <span data-ttu-id="33147-150">Vyberte **zrušit** v horní části **čekající na schválení** okno.</span><span class="sxs-lookup"><span data-stu-id="33147-150">Select **Cancel** at the top of the **Pending approval** blade.</span></span>

   ![Zrušit čekající na vyřízení žádosti – snímek obrazovky][4]

## <a name="next-steps"></a><span data-ttu-id="33147-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33147-152">Next steps</span></span>
<span data-ttu-id="33147-153">Pokud byste chtěli dozvědět více o Azure AD Privileged Identity Management, tyto odkazy obsahovat další informace.</span><span class="sxs-lookup"><span data-stu-id="33147-153">If you're interested in learning more about Azure AD Privileged Identity Management, the following links have more information.</span></span>

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
