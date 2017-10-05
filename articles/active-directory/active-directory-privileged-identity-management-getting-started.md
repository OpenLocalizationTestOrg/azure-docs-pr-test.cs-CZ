---
title: "Začínáme s aplikací Azure AD Privileged Identity Management | Dokumentace Microsoftu"
description: "Přečtěte si, jak spravovat privilegované identity pomocí aplikace Azure Active Directory Privileged Identity Management na webu Azure Portal."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a><span data-ttu-id="78013-103">Začátek práce s aplikací Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="78013-103">Start using Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="78013-104">Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="78013-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="78013-105">Tento obor zahrnuje přístup k prostředkům ve službě Azure AD a dalších online službách Microsoftu jako Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="78013-105">This scope includes access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="78013-106">Tento článek vysvětluje, jak přidat aplikaci Azure AD PIM do řídicího panelu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="78013-106">This article tells you how to add the Azure AD PIM app to your Azure portal dashboard.</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="78013-107">Přidání aplikace Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="78013-107">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="78013-108">Před použitím aplikace Azure AD Privileged Identity Management musíte tuto aplikaci přidat do řídicího panelu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="78013-108">Before you use Azure AD Privileged Identity Management, you need to add the application to your Azure portal dashboard.</span></span>

1. <span data-ttu-id="78013-109">Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="78013-109">Sign in to the [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="78013-110">Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="78013-110">If your organization has more than one directory, select your username in the upper right-hand corner of the Azure portal.</span></span> <span data-ttu-id="78013-111">Vyberte adresář, kde chcete PIM používat.</span><span class="sxs-lookup"><span data-stu-id="78013-111">Select the directory where you want to use PIM.</span></span>
3. <span data-ttu-id="78013-112">Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="78013-112">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="78013-113">Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="78013-113">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="78013-114">Aplikace Privileged Identity Management se otevře.</span><span class="sxs-lookup"><span data-stu-id="78013-114">The Privileged Identity Management application opens.</span></span>

<span data-ttu-id="78013-115">Pokud jste první, kdo ve vašem adresáři používá Azure AD Privileged Identity Management, automaticky se vám přiřadí role **Správce zabezpečení** a **Správce privilegovaných rolí** v daném adresáři.</span><span class="sxs-lookup"><span data-stu-id="78013-115">If you're the first person to use Azure AD Privileged Identity Management in your directory, you are automatically assigned the **Security administrator** and **Privileged role administrator** roles in the directory.</span></span> <span data-ttu-id="78013-116">Pouze správci privilegovaných rolí můžou spravovat přiřazení rolí uživatelů.</span><span class="sxs-lookup"><span data-stu-id="78013-116">Only privileged role administrators can manage role assignments of users.</span></span> <span data-ttu-id="78013-117">Kromě toho můžete spustit [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md),</span><span class="sxs-lookup"><span data-stu-id="78013-117">In addition, you may choose to run the [security wizard.](active-directory-privileged-identity-management-security-wizard.md)</span></span> <span data-ttu-id="78013-118">který vás provede počátečním zjišťováním a přiřazováním.</span><span class="sxs-lookup"><span data-stu-id="78013-118">that walks you through the initial discovery and assignment experience.</span></span>

## <a name="navigate-to-your-tasks"></a><span data-ttu-id="78013-119">Přechod k úkolům</span><span class="sxs-lookup"><span data-stu-id="78013-119">Navigate to your tasks</span></span>
<span data-ttu-id="78013-120">Po nastavení Azure AD Privileged Identity Management se vždy, když aplikaci otevřete, zobrazí navigační okno.</span><span class="sxs-lookup"><span data-stu-id="78013-120">Once Azure AD Privileged Identity Management is set up, you see the navigation blade whenever you open the application.</span></span> <span data-ttu-id="78013-121">Pomocí tohoto okna provádějte úkoly správy identity.</span><span class="sxs-lookup"><span data-stu-id="78013-121">Use this blade to accomplish your identity management tasks.</span></span>

![Úkoly nejvyšší úrovně v PIM – snímek obrazovky](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* <span data-ttu-id="78013-123">**Moje role** – přejdete k seznamu rolí, které vám jsou přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="78013-123">**My Roles** takes you to a list of roles that are assigned to you.</span></span> <span data-ttu-id="78013-124">V této části můžete aktivovat role, ke kterým máte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="78013-124">This section is where you activate any roles that you are eligible for.</span></span>
* <span data-ttu-id="78013-125">**Schválit žádosti (Preview)** zobrazí seznam žádostí uživatelů ve vašem adresáři o aktivaci, které čekají na schválení.</span><span class="sxs-lookup"><span data-stu-id="78013-125">**Approve Requests (Preview)** displays a list of pending activation requests from users in your directory.</span></span> [<span data-ttu-id="78013-126">Další informace</span><span class="sxs-lookup"><span data-stu-id="78013-126">Learn more.</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* <span data-ttu-id="78013-127">**Čekající žádosti (Preview)** zobrazí všechny aktuální žádosti k aktivaci.</span><span class="sxs-lookup"><span data-stu-id="78013-127">**Pending Requests (Preview)** displays any current requests to have made to activate.</span></span>
* <span data-ttu-id="78013-128">**Zkontrolovat přístup** – přejdete k čekajícím kontrolám přístupu, které je potřeba dokončit, ať už kontrolujete přístup sobě nebo někomu jinému.</span><span class="sxs-lookup"><span data-stu-id="78013-128">**Review Access** takes you to any pending access reviews that you need to complete, whether you're reviewing access for yourself or someone else.</span></span>
* <span data-ttu-id="78013-129">**Role adresáře služby Azure AD** v části Správa je řídicí panel pro správce privilegovaných rolí, odkud se dá spravovat přiřazení rolí, měnit nastavení aktivace rolí, začít kontrolu přístupu atd.</span><span class="sxs-lookup"><span data-stu-id="78013-129">**Azure AD Directory Roles** located under the 'Manage' section is the dashboard for privileged role admins to manage role assignments, change role activation settings, start access reviews, and more.</span></span> <span data-ttu-id="78013-130">Možnosti na tomto řídicím panelu se zobrazují jen správcům privilegovaných rolí.</span><span class="sxs-lookup"><span data-stu-id="78013-130">The options in this dashboard are disabled for anyone who isn't a privileged role administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78013-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78013-131">Next steps</span></span>
<span data-ttu-id="78013-132">[Přehled aplikace Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) obsahuje podrobné informace o tom, jak můžete spravovat přístup pro správu ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="78013-132">The [Azure AD Privileged Identity Management overview](active-directory-privileged-identity-management-configure.md) includes more details on how you can manage administrative access in your organization.</span></span>

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
