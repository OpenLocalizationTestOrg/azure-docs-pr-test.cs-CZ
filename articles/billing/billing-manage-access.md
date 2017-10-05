---
title: "Správa přístupu k Azure fakturace použití rolí | Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: c70904097f139bc2178feed83f1cf1274f3c738d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="d53e1-102">Správa přístupu k fakturační informace pro Azure pomocí řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="d53e1-102">Manage access to billing information for Azure using role-based access control</span></span>

<span data-ttu-id="d53e1-103">Můžete udělit přístup Azure fakturační informace pro členy týmu přiřazením jednu z následujících rolí uživatele k předplatnému: správce účtu, Správce služby, spolusprávcem, vlastník, Přispěvatel, Čtenář a fakturace čtečky.</span><span class="sxs-lookup"><span data-stu-id="d53e1-103">You can grant access for Azure billing information to members of your team by assigning one of the following user roles to your subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="d53e1-104">By mají přístup k fakturační informace v [portál Azure](https://portal.azure.com/), a mohou používat [fakturace rozhraní API](billing-usage-rate-card-overview.md) prostřednictvím kódu programu získat faktury (jednou přihlásí) a podrobnosti o použití.</span><span class="sxs-lookup"><span data-stu-id="d53e1-104">They would have access to billing information in the [Azure portal](https://portal.azure.com/), and they can use the [Billing APIs](billing-usage-rate-card-overview.md) to programmatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="d53e1-105">Další informace o, který můžete udělit role, a které role co naleznete na adrese [role v Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="d53e1-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="d53e1-106"><a name="opt-in"></a>Povolíte dalším uživatelům přístup k faktury</span><span class="sxs-lookup"><span data-stu-id="d53e1-106"><a name="opt-in"></a> Allowing additional users to access invoices</span></span>

<span data-ttu-id="d53e1-107">Účet správce musí povolit pomocí [portál Azure](https://portal.azure.com/) povolit přístup k faktury pro ostatní uživatele nebo přes rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d53e1-107">The Account Administrator must opt in using the [Azure portal](https://portal.azure.com/) allow access to invoices for other users and via API.</span></span>

1. <span data-ttu-id="d53e1-108">Jako správce účtu, vyberte své předplatné z [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-108">As the Account Administrator, select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="d53e1-109">Vyberte **faktury** a potom **přístup k faktury**.</span><span class="sxs-lookup"><span data-stu-id="d53e1-109">Select **Invoices** and then **Access to invoices**.</span></span>

    ![Snímek obrazovky ukazuje, jak delegovat přístup k faktury](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="d53e1-111">Zapnout **na** přístup, za nímž následuje uložením změn, aby uživatelé v rámci předplatného obor role ke stažení faktury.</span><span class="sxs-lookup"><span data-stu-id="d53e1-111">Turn **On** the access followed by saving the changes, to allow users in subscription scoped roles to download invoice.</span></span>

    ![Snímek obrazovky ukazuje zapnutí a vypnutí delegovat přístup k fakturaci](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="d53e1-113">Výslovným souhlasem umožňuje služby správce, spolusprávce, vlastník, Přispěvatel, Čtenář a fakturace čtečky u předplatného ke stažení faktury PDF na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on the subscription to download PDF invoices in the Azure portal.</span></span> <span data-ttu-id="d53e1-114">Faktury starší než prosinec 2016 jsou však k dispozici pouze pro účet správce teď.</span><span class="sxs-lookup"><span data-stu-id="d53e1-114">However, invoices older than December 2016 are available only to the Account Administrator for now.</span></span>

<span data-ttu-id="d53e1-115">Správce účtu můžete taky nakonfigurovat tak, aby měl faktury odesílaly e-mailem.</span><span class="sxs-lookup"><span data-stu-id="d53e1-115">The Account Administrator can also configure to have invoices sent via email.</span></span> <span data-ttu-id="d53e1-116">Další informace najdete v tématu [získat faktury v e-mailu](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="d53e1-116">To learn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-to-the-billing-reader-role"></a><span data-ttu-id="d53e1-117">Přidání uživatelů do role Čtenář fakturace</span><span class="sxs-lookup"><span data-stu-id="d53e1-117">Adding users to the Billing Reader role</span></span>

<span data-ttu-id="d53e1-118">Role čtenáře fakturace má přístup jen pro čtení k fakturační informace o odběru na portálu Azure a žádný přístup ke službám, jako je například virtuální počítače a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d53e1-118">The Billing Reader role has read-only access to subscription billing information in Azure portal, and no access to services such as VMs and storage accounts.</span></span> <span data-ttu-id="d53e1-119">Přiřadíte role Čtenář fakturace jinému uživateli, který potřebuje přístup k fakturační informace o předplatném, ale není možnost spravovat služby Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-119">Assign the Billing Reader role to someone that needs access to the subscription billing information but not the ability to manage Azure services.</span></span> <span data-ttu-id="d53e1-120">Tato role je vhodný pro uživatele v organizaci, kteří pouze provádět správu finanční a nákladů pro předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="d53e1-121">Vyberte své předplatné z [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-121">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="d53e1-122">Vyberte **přístup k ovládacímu prvku (IAM)** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d53e1-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Snímek obrazovky ukazuje IAM v okně předplatného](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="d53e1-124">Zvolte **fakturace čtečky** v **vyberte roli** stránky.</span><span class="sxs-lookup"><span data-stu-id="d53e1-124">Choose **Billing Reader** in the **Select a role** page.</span></span>

    ![Snímek obrazovky ukazuje fakturace čtečky v automaticky otevřeném okně zobrazení](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="d53e1-126">Zadejte e-mailu pro uživatele, které chcete pozvat a pak klikněte na **OK** odeslat pozvánky.</span><span class="sxs-lookup"><span data-stu-id="d53e1-126">Type the email for the user you want to invite, then click **OK** to send the invitation.</span></span>

    ![Snímek obrazovky zobrazující k zadání e-mailu pozvání uživatele](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="d53e1-128">Postupujte podle pokynů v e-mailu pozvání k přihlášení jako čtečku fakturace.</span><span class="sxs-lookup"><span data-stu-id="d53e1-128">Follow instructions in the invite email to log in as a Billing Reader.</span></span>

    ![Snímek obrazovky, který znázorňuje co čtečky fakturace můžete zobrazit na portálu Azure](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="d53e1-130">Funkci fakturace čtečky je ve verzi preview a zatím nepodporuje předplatného typu enterprise (EA) nebo neglobální cloudy.</span><span class="sxs-lookup"><span data-stu-id="d53e1-130">The Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-to-other-roles"></a><span data-ttu-id="d53e1-131">Přidání uživatelů k jiné role</span><span class="sxs-lookup"><span data-stu-id="d53e1-131">Adding users to other roles</span></span>

<span data-ttu-id="d53e1-132">Uživatelé v jiných rolí, například vlastníka nebo přispěvatele, můžou používat jenom fakturační informace, ale také služby Azure.</span><span class="sxs-lookup"><span data-stu-id="d53e1-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="d53e1-133">Ke správě těchto rolí, najdete v části [přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="d53e1-133">To manage these roles, see [Add or change Azure administrator roles that manage the subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="d53e1-134">Kdo má přístup [centra účtů](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="d53e1-134">Who can access the [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="d53e1-135">Pouze správce účtu může Přihlaste se k centru účtů.</span><span class="sxs-lookup"><span data-stu-id="d53e1-135">Only the Account Administrator can log in to the Account center.</span></span> <span data-ttu-id="d53e1-136">Správce účtu je právní vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="d53e1-136">The Account Administrator is the legal owner of the subscription.</span></span> <span data-ttu-id="d53e1-137">Ve výchozím nastavení, osoba, která registraci aplikace nebo kód zakoupili předplatné Azure je účet správce, pokud [byl přenos vlastnictví předplatného](billing-subscription-transfer.md) pošlete někomu jinému.</span><span class="sxs-lookup"><span data-stu-id="d53e1-137">By default, the person who signed up for or bought the Azure subscription is the Account Administrator, unless the [subscription ownership was transferred](billing-subscription-transfer.md) to somebody else.</span></span> <span data-ttu-id="d53e1-138">Správce účtu můžete vytvářet odběry, zrušit předplatné, změňte adresu fakturace předplatného a spravovat zásady přístupu pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="d53e1-138">The Account Administrator can create subscriptions, cancel subscriptions, change the billing address for a subscription, and manage access policies for the subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="d53e1-139">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="d53e1-139">Need help?</span></span> <span data-ttu-id="d53e1-140">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="d53e1-140">Contact support.</span></span>

<span data-ttu-id="d53e1-141">Pokud máte další otázky, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="d53e1-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
