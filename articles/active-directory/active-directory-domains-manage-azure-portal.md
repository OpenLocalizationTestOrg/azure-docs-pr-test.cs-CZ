---
title: "Správa vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Koncepty správy a postupy pro správu název domény v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: curtand;jeffsta
ms.openlocfilehash: 402c1be07b8ee885ee5341128fb3f419611b924d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="7ca94-103">Správa vlastních názvů domén v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ca94-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="7ca94-104">Název domény, je důležitou součástí identifikátoru pro mnoho prostředků adresáře: je součástí uživatelského jména nebo e-mailové adresy pro uživatele, část adresy pro skupinu a můžou být součástí identifikátor ID URI aplikace pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7ca94-104">A domain name is an important part of the identifier for many directory resources: it is part of a user name or email address for a user, part of the address for a group, and can be part of the app ID URI for an application.</span></span> <span data-ttu-id="7ca94-105">Prostředek v Azure Active Directory (Azure AD) může zahrnovat název domény, který už je ověřený jako vlastníkem adresáře, která obsahuje daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="7ca94-105">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified as owned by the directory that contains the resource.</span></span> <span data-ttu-id="7ca94-106">Globální správce můžete provádět úlohy správy domény ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ca94-106">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="7ca94-107">Nastavte název primární domény pro váš adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca94-107">Set the primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="7ca94-108">Při vytváření adresáře, název domény, jako je například 'contoso.onmicrosoft.com', je také název primární doménu.</span><span class="sxs-lookup"><span data-stu-id="7ca94-108">When your directory is created, the initial domain name, such as ‘contoso.onmicrosoft.com,’ is also the primary domain name.</span></span> <span data-ttu-id="7ca94-109">Primární doména je výchozí název domény pro nového uživatele při vytváření nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ca94-109">The primary domain is the default domain name for a new user when you create a new user.</span></span> <span data-ttu-id="7ca94-110">Nastavení primární doménu zjednodušuje proces pro správce k vytváření nových uživatelů na portálu.</span><span class="sxs-lookup"><span data-stu-id="7ca94-110">Setting a primary domain name streamlines the process for an administrator to create new users in the portal.</span></span> <span data-ttu-id="7ca94-111">Chcete-li změnit název primární domény:</span><span class="sxs-lookup"><span data-stu-id="7ca94-111">To change the primary domain name:</span></span>

1. <span data-ttu-id="7ca94-112">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="7ca94-112">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="7ca94-113">Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="7ca94-113">Select **More services**, enter **Azure Active Directory** in the text box, and then select **Enter**.</span></span>
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. <span data-ttu-id="7ca94-115">Na ***název adresáře*** vyberte **názvy domén**.</span><span class="sxs-lookup"><span data-stu-id="7ca94-115">On the ***directory-name*** blade, select **Domain names**.</span></span>
4. <span data-ttu-id="7ca94-116">Na  ***název adresáře* -názvy domén** okně, vyberte možnost názvu domény, kterou chcete nastavit název primární domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-116">On the ***directory-name* - Domain names** blade, select the domain name you would like to make the primary domain name.</span></span>
5. <span data-ttu-id="7ca94-117">Na ***název domény*** okno (to znamená, v okně otevřeném obsahující v názvu nový název domény), vyberte **nastavit jako primární** příkaz.</span><span class="sxs-lookup"><span data-stu-id="7ca94-117">On the ***domain name*** blade (that is, the blade that opens that has your new domain name in the title), select the **Make primary** command.</span></span> <span data-ttu-id="7ca94-118">Potvrďte volbu po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="7ca94-118">Confirm your choice when prompted.</span></span>
   
   ![Zkontrolujte název domény, primární](./media/active-directory-domains-manage-azure-portal/make-primary.png)

<span data-ttu-id="7ca94-120">Můžete změnit název primární domény pro váš adresář na všechny ověřené vlastní doménu, která není federovaný.</span><span class="sxs-lookup"><span data-stu-id="7ca94-120">You can change the primary domain name for your directory to be any verified custom domain that is not federated.</span></span> <span data-ttu-id="7ca94-121">Změna primární domény pro váš adresář nedojde ke změně uživatelská jména pro všechny stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ca94-121">Changing the primary domain for your directory will not change the user names for any existing users.</span></span>

## <a name="add-custom-domain-names-to-your-azure-ad"></a><span data-ttu-id="7ca94-122">Přidat vlastní názvy domén do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca94-122">Add custom domain names to your Azure AD</span></span>
<span data-ttu-id="7ca94-123">Pro každý adresář Azure AD můžete přidat až 900 názvy vlastních domén.</span><span class="sxs-lookup"><span data-stu-id="7ca94-123">You can add up to 900 custom domain names to each Azure AD directory.</span></span> <span data-ttu-id="7ca94-124">Proces [, přidejte další vlastní domény název](add-custom-domain.md) je stejný pro první vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-124">The process to [add an additional custom domain name](add-custom-domain.md) is the same for the first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="7ca94-125">Přidat subdomény vlastní domény</span><span class="sxs-lookup"><span data-stu-id="7ca94-125">Add subdomains of a custom domain</span></span>
<span data-ttu-id="7ca94-126">Pokud chcete přidat název domény třetí úrovně například 'europe.contoso.com' do vašeho adresáře, měli byste nejprve přidat a ověřit domény druhé úrovně, například contoso.com. Subdoméně bude automaticky ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ca94-126">If you want to add a third-level domain name such as ‘europe.contoso.com’ to your directory, you should first add and verify the second-level domain, such as contoso.com. The subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="7ca94-127">Pokud chcete zjistit, zda byla ověřena subdomény, kterou jste právě přidali, aktualizujte stránku v prohlížeči, který obsahuje seznam domén.</span><span class="sxs-lookup"><span data-stu-id="7ca94-127">To see that the subdomain that you just added has been verified, refresh the page in the browser that lists the domains.</span></span>

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="7ca94-128">Co dělat, když změníte registrátora DNS pro vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="7ca94-128">What to do if you change the DNS registrar for your custom domain name</span></span>
<span data-ttu-id="7ca94-129">Pokud změníte registrátora DNS pro vlastní název domény, můžete nadále používat vlastní název domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy.</span><span class="sxs-lookup"><span data-stu-id="7ca94-129">If you change the DNS registrar for your custom domain name, you can continue to use your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="7ca94-130">Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci pro tyto služby.</span><span class="sxs-lookup"><span data-stu-id="7ca94-130">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer to the documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="7ca94-131">Odstranit vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="7ca94-131">Delete a custom domain name</span></span>
<span data-ttu-id="7ca94-132">Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud budete muset použít název domény s jinou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ca94-132">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need to use that domain name with another Azure AD.</span></span>

<span data-ttu-id="7ca94-133">Chcete-li odstranit vlastní název domény, musíte nejdřív zkontrolovat, že žádné prostředky ve vašem adresáři spoléhají na název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-133">To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name.</span></span> <span data-ttu-id="7ca94-134">Název domény nelze odstranit z adresáře, pokud:</span><span class="sxs-lookup"><span data-stu-id="7ca94-134">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="7ca94-135">Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-135">Any user has a user name, email address, or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="7ca94-136">Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-136">Any group has an email address or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="7ca94-137">Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-137">Any application in your Azure AD has an app ID URI that includes the domain name.</span></span>

<span data-ttu-id="7ca94-138">Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, chcete-li odstranit vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="7ca94-138">You must change or delete any such resource in your Azure AD directory before you can delete the custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a><span data-ttu-id="7ca94-139">Použijte PowerShell nebo rozhraní Graph API, které slouží ke správě názvů domén</span><span class="sxs-lookup"><span data-stu-id="7ca94-139">Use PowerShell or Graph API to manage domain names</span></span>
<span data-ttu-id="7ca94-140">Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="7ca94-140">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="7ca94-141">Použití Powershellu ke správě názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca94-141">Using PowerShell to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="7ca94-142">Pomocí rozhraní Graph API ke správě názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca94-142">Using Graph API to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="7ca94-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ca94-143">Next steps</span></span>
* [<span data-ttu-id="7ca94-144">Přidat vlastní názvy domén</span><span class="sxs-lookup"><span data-stu-id="7ca94-144">Add custom domain names</span></span>](add-custom-domain.md)

