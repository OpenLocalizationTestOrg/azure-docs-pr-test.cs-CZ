---
title: "Postup propojení předplatné Azure s Azure AD B2C | Microsoft Docs"
description: "Podrobný průvodce povolit fakturace pro klienta Azure AD B2C do předplatného Azure."
services: active-directory-b2c
documentationcenter: dev-center-name
author: parakhj
manager: mtillman
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2017
ms.author: parja
ms.openlocfilehash: 063c00fe47be25b9359e80d71abfaf453c7a7074
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="linking-an-azure-subscription-to-an-azure-ad-b2c-tenant"></a><span data-ttu-id="2acb2-103">Propojování předplatné Azure klienta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2acb2-103">Linking an Azure Subscription to an Azure AD B2C tenant</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2acb2-104">Nejnovější informace o využití fakturaci a cenách pro Azure AD B2C je na následující stránce: [cenová Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)</span><span class="sxs-lookup"><span data-stu-id="2acb2-104">The latest information on usage billing and pricing for Azure AD B2C is at the following page: [Azure AD B2C Pricing](https://azure.microsoft.com/pricing/details/active-directory-b2c/)</span></span>

<span data-ttu-id="2acb2-105">K předplatnému Azure se účtují poplatky za používání pro Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-105">Usage charges for Azure AD B2C are billed to an Azure subscription.</span></span> <span data-ttu-id="2acb2-106">Při vytváření klienta služby Azure AD B2C, musí správce klienta explicitně propojení klienta Azure AD B2C k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-106">When an Azure AD B2C tenant is created, the tenant administrator needs to explicitly link the Azure AD B2C tenant to an Azure subscription.</span></span> <span data-ttu-id="2acb2-107">Tento článek ukazuje, jak.</span><span class="sxs-lookup"><span data-stu-id="2acb2-107">This article shows you how.</span></span>

> [!NOTE]
> <span data-ttu-id="2acb2-108">Předplatné propojené s klienta Azure AD B2C můžete použít pouze pro fakturaci využití Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-108">A subscription linked to an Azure AD B2C tenant can only be used for the billing of Azure AD B2C usage.</span></span> <span data-ttu-id="2acb2-109">Odběr nelze použít k přidání další služby Azure nebo Office 365 licence *v rámci klienta Azure AD B2C*.</span><span class="sxs-lookup"><span data-stu-id="2acb2-109">The subscription cannot be used to add other Azure services or Office 365 licenses *within the Azure AD B2C tenant*.</span></span>

 <span data-ttu-id="2acb2-110">Odkaz pro předplatné je dosaženo vytvořením Azure AD B2C "prostředek" v rámci cílové předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-110">The subscription link is achieved by creating an Azure AD B2C "resource" within the target Azure subscription.</span></span> <span data-ttu-id="2acb2-111">V rámci jednoho předplatného Azure, společně s dalším prostředkům služby Azure (pro virtuální počítače, například dat úložiště, LogicApps) můžete vytvořit mnoho Azure AD B2C "zdroje".</span><span class="sxs-lookup"><span data-stu-id="2acb2-111">Many Azure AD B2C "resources" can be created within a single Azure subscription, along with other Azure resources (for example, VMs, Data storage, LogicApps).</span></span> <span data-ttu-id="2acb2-112">Uvidíte všechny prostředky v rámci předplatného na klienta služby Azure AD, který je předplatné přidružené k webu.</span><span class="sxs-lookup"><span data-stu-id="2acb2-112">You can see all of the resources within the subscription by going to the Azure AD tenant that the subscription is associated to.</span></span>

<span data-ttu-id="2acb2-113">Chcete-li pokračovat, je potřeba platné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-113">A valid Azure subscription is needed to proceed.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="2acb2-114">Vytvoření tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2acb2-114">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="2acb2-115">Je nutné nejprve [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md) , které chcete propojit předplatné.</span><span class="sxs-lookup"><span data-stu-id="2acb2-115">You must first [create an Azure AD B2C tenant](active-directory-b2c-get-started.md) that you would like to link a subscription to.</span></span> <span data-ttu-id="2acb2-116">Tento krok přeskočte, pokud jste již vytvořili klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-116">Skip this step if you have already created an Azure AD B2C tenant.</span></span>

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a><span data-ttu-id="2acb2-117">Otevřete portál Azure v klientovi Azure AD, který ukazuje vašeho předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="2acb2-117">Open Azure portal in the Azure AD tenant that shows your Azure subscription</span></span>

<span data-ttu-id="2acb2-118">Přejděte do klienta Azure AD, který ukazuje vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-118">Navigate to the Azure AD tenant that shows your Azure subscription.</span></span> <span data-ttu-id="2acb2-119">Otevřete [portál Azure](https://portal.azure.com)a přejděte na klienta Azure AD, který ukazuje předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2acb2-119">Open the [Azure portal](https://portal.azure.com), and switch to the Azure AD tenant that shows the Azure subscription you would like to use.</span></span>

![Přepnutí na klientovi Azure AD](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a><span data-ttu-id="2acb2-121">Najít Azure AD B2C v Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="2acb2-121">Find Azure AD B2C in the Azure Marketplace</span></span>

<span data-ttu-id="2acb2-122">Klikněte na tlačítko **Nový**.</span><span class="sxs-lookup"><span data-stu-id="2acb2-122">Click the **New** button.</span></span> <span data-ttu-id="2acb2-123">Do pole **Hledat na Marketplace** zadejte `B2C`.</span><span class="sxs-lookup"><span data-stu-id="2acb2-123">In the **Search the marketplace** field, enter `B2C`.</span></span>

![Přidejte tlačítkem a text Azure AD B2C ve vyhledávání pole marketplace.](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

<span data-ttu-id="2acb2-125">V seznamu výsledků vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="2acb2-125">In the results list, select **Azure AD B2C**.</span></span>

![Azure AD B2C vybraného v seznamu výsledků](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

<span data-ttu-id="2acb2-127">Jsou uvedeny podrobnosti o Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-127">Details about Azure AD B2C are shown.</span></span> <span data-ttu-id="2acb2-128">Pokud chcete začít konfigurovat nového tenanta Azure Active Directory B2C, klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2acb2-128">To begin configuring your new Azure Active Directory B2C tenant, click the **Create** button.</span></span>

<span data-ttu-id="2acb2-129">Na obrazovce vytváření prostředků vyberte **odkaz existující Azure AD B2C klienta pro Moje předplatné**.</span><span class="sxs-lookup"><span data-stu-id="2acb2-129">In the resource creation screen, select **Link an existing Azure AD B2C Tenant to my Azure subscription**.</span></span>

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a><span data-ttu-id="2acb2-130">Vytvořit prostředek služby Azure AD B2C v rámci předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="2acb2-130">Create an Azure AD B2C resource within the Azure subscription</span></span>

<span data-ttu-id="2acb2-131">V dialogovém okně vytváření prostředků vyberte z rozevíracího seznamu klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-131">In the resource creation dialog, select an Azure AD B2C tenant from the dropdown.</span></span> <span data-ttu-id="2acb2-132">Zobrazí se všechny klienty, kteří jsou globální správce a ty, které již nejsou připojeny k odběru.</span><span class="sxs-lookup"><span data-stu-id="2acb2-132">You will see all of the tenants that you are the global administrator of and those that are not already linked to a subscription.</span></span>

<span data-ttu-id="2acb2-133">Název prostředku Azure AD B2C bude být předvoleno podle názvu domény klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-133">The Azure AD B2C resource name will be preselected to match the domain name of the Azure AD B2C tenant.</span></span>

<span data-ttu-id="2acb2-134">Pro předplatné vyberte aktivní předplatné Azure, které jsou oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="2acb2-134">For Subscription, select an active Azure subscription that you are the administrator of.</span></span>

<span data-ttu-id="2acb2-135">Vyberte skupinu prostředků a umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2acb2-135">Select a Resource Group and Resource Group location.</span></span> <span data-ttu-id="2acb2-136">Výběr zde nemá žádný vliv na umístění klienta Azure AD B2C, výkon nebo stav fakturace.</span><span class="sxs-lookup"><span data-stu-id="2acb2-136">The selection here has no impact on your Azure AD B2C tenant location, performance, or billing status.</span></span>

![Vytvořte prostředek B2C](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a><span data-ttu-id="2acb2-138">Spravovat prostředky klienta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2acb2-138">Manage your Azure AD B2C tenant resources</span></span>

<span data-ttu-id="2acb2-139">Po úspěšném vytvoření prostředek služby Azure AD B2C v rámci předplatného Azure, měli byste vidět nový prostředek typu "Klienta B2C" přidat spolu s vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-139">Once an Azure AD B2C resource is successfully created within the Azure subscription, you should see a new resource of the type "B2C tenant" added alongside your other Azure resources.</span></span>

<span data-ttu-id="2acb2-140">Můžete použít tento prostředek na:</span><span class="sxs-lookup"><span data-stu-id="2acb2-140">You can use this resource to:</span></span>

- <span data-ttu-id="2acb2-141">Přejděte na předplatné si projděte fakturační informace.</span><span class="sxs-lookup"><span data-stu-id="2acb2-141">Navigate to the subscription to review billing information.</span></span>
- <span data-ttu-id="2acb2-142">Přejděte ke klientovi Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="2acb2-142">Go to your Azure AD B2C tenant</span></span>
- <span data-ttu-id="2acb2-143">Odeslání žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="2acb2-143">Submit a support request</span></span>
- <span data-ttu-id="2acb2-144">Přesunutí prostředku klienta Azure AD B2C, do jiného předplatného Azure nebo do jiné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2acb2-144">Move your Azure AD B2C tenant resource to another Azure subscription or to another Resource Group.</span></span>

![Nastavení prostředků B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a><span data-ttu-id="2acb2-146">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="2acb2-146">Known Issues</span></span>

### <a name="csp-subscriptions"></a><span data-ttu-id="2acb2-147">Odběry zprostředkovatele kryptografických služeb</span><span class="sxs-lookup"><span data-stu-id="2acb2-147">CSP Subscriptions</span></span>

<span data-ttu-id="2acb2-148">V současné době klienta Azure AD B2C **nelze** odkaz na předplatná CSP.</span><span class="sxs-lookup"><span data-stu-id="2acb2-148">Currently, an Azure AD B2C tenant **cannot** link to CSP subscriptions.</span></span>

### <a name="self-imposed-restrictions"></a><span data-ttu-id="2acb2-149">Samoobslužné uložena omezení</span><span class="sxs-lookup"><span data-stu-id="2acb2-149">Self-imposed restrictions</span></span>

<span data-ttu-id="2acb2-150">Uživatel může vytvořit místní omezení pro vytváření prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-150">A user may have established a regional restriction for Azure resource creation.</span></span> <span data-ttu-id="2acb2-151">Toto omezení by mohlo zabránit vytvoření prostředku Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="2acb2-151">This restriction may prevent the creation of Azure AD B2C resource.</span></span> <span data-ttu-id="2acb2-152">Pokud chcete zmírnit, prosím toto omezení zmírnit.</span><span class="sxs-lookup"><span data-stu-id="2acb2-152">To mitigate, please relax this restriction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2acb2-153">Další postup</span><span class="sxs-lookup"><span data-stu-id="2acb2-153">Next steps</span></span>

<span data-ttu-id="2acb2-154">Po dokončení pro jednotlivé klienty Azure AD B2C těchto kroků se vašeho předplatného Azure se fakturuje v souladu s podrobností o Azure přímý nebo smlouvu Enterprise.</span><span class="sxs-lookup"><span data-stu-id="2acb2-154">Once these steps are complete for each of your Azure AD B2C tenants, your Azure subscription is billed in accordance with your Azure Direct or Enterprise Agreement details.</span></span>

<span data-ttu-id="2acb2-155">Využití a fakturačních údajů můžete zkontrolovat ve vašem vybraném předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="2acb2-155">You can review the usage and billing details within your selected Azure subscription.</span></span> <span data-ttu-id="2acb2-156">Můžete rovněž zkontrolovat podrobné informace o použití ve dnech sestav pomocí [využití reporting rozhraní API](active-directory-b2c-reference-usage-reporting-api.md).</span><span class="sxs-lookup"><span data-stu-id="2acb2-156">You can also review detailed day-by-day usage reports using the [usage reporting API](active-directory-b2c-reference-usage-reporting-api.md).</span></span>
