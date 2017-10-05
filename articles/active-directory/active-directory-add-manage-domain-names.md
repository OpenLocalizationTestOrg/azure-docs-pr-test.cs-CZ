---
title: "Správa vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Koncepty správy a postupy pro správu vlastní doménu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="2c9f4-103">Správa vlastních názvů domén v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c9f4-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="2c9f4-104">Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:</span><span class="sxs-lookup"><span data-stu-id="2c9f4-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="2c9f4-105">Pro uživatele uživatelského jména nebo e-mailové adresy</span><span class="sxs-lookup"><span data-stu-id="2c9f4-105">A user name or email address for a user</span></span>
* <span data-ttu-id="2c9f4-106">Adresa pro skupinu</span><span class="sxs-lookup"><span data-stu-id="2c9f4-106">The address for a group</span></span>
* <span data-ttu-id="2c9f4-107">Identifikátor ID URI aplikace pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="2c9f4-107">The app ID URI for an application</span></span>

<span data-ttu-id="2c9f4-108">Prostředek v Azure Active Directory (Azure AD) může zahrnovat název domény, který je již ověřit vlastnit adresář, který obsahuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified to be owned by the directory that contains the resource.</span></span> <span data-ttu-id="2c9f4-109">Globální správce můžete provádět úlohy správy domény ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c9f4-110">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-110">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="2c9f4-111">Jak spravovat názvy domén v Centru správy služby Azure AD, najdete v článku [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2c9f4-111">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="2c9f4-112">Nastavte název primární domény pro váš adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c9f4-112">Set the primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="2c9f4-113">Při vytváření adresáře, název domény, jako je například 'contoso.onmicrosoft.com', je také název primární domény pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-113">When your directory is created, the initial domain name, such as ‘contoso.onmicrosoft.com,’ is also the primary domain name for your directory.</span></span> <span data-ttu-id="2c9f4-114">Primární doména je výchozí název domény pro nového uživatele, když vytvoříte nový uživatel v [portál Azure classic](https://manage.windowsazure.com/), nebo dalších portálech, jako je například portál pro správu Office 365.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-114">The primary domain is the default domain name for a new user when you create a new user in the [Azure classic portal](https://manage.windowsazure.com/), or other portals such as the Office 365 admin portal.</span></span> <span data-ttu-id="2c9f4-115">To zjednodušuje proces pro správce k vytváření nových uživatelů na portálu.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-115">This streamlines the process for an administrator to create new users in the portal.</span></span>

<span data-ttu-id="2c9f4-116">Chcete-li změnit název primární domény pro váš adresář:</span><span class="sxs-lookup"><span data-stu-id="2c9f4-116">To change the primary domain name for your directory:</span></span>

1. <span data-ttu-id="2c9f4-117">Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-117">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="2c9f4-118">Vyberte **služby Active Directory** na levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-118">Select **Active Directory** on the left navigation bar.</span></span>
3. <span data-ttu-id="2c9f4-119">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-119">Open your directory.</span></span>
4. <span data-ttu-id="2c9f4-120">Vyberte **domén** kartě.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-120">Select the **Domains** tab.</span></span>
5. <span data-ttu-id="2c9f4-121">Vyberte **změnu primární** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-121">Select the **Change primary** button on the command bar.</span></span>
6. <span data-ttu-id="2c9f4-122">Vyberte doménu, která mají být nové primární domény pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-122">Select the domain that you want to be the new primary domain for your directory.</span></span>

<span data-ttu-id="2c9f4-123">Můžete změnit název primární domény pro váš adresář na všechny ověřené vlastní doménu, která není federovaný.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-123">You can change the primary domain name for your directory to be any verified custom domain that is not federated.</span></span> <span data-ttu-id="2c9f4-124">Změna primární domény pro váš adresář nedojde ke změně uživatelská jména pro všechny stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-124">Changing the primary domain for your directory will not change the user names for any existing users.</span></span>

## <a name="add-custom-domain-names-to-your-azure-ad"></a><span data-ttu-id="2c9f4-125">Přidat vlastní názvy domén do služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c9f4-125">Add custom domain names to your Azure AD</span></span>
<span data-ttu-id="2c9f4-126">Pro každý adresář Azure AD můžete přidat až 900 názvy vlastních domén.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-126">You can add up to 900 custom domain names to each Azure AD directory.</span></span> <span data-ttu-id="2c9f4-127">Proces [, přidejte další vlastní domény název](active-directory-add-domain.md) je stejný pro první vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-127">The process to [add an additional custom domain name](active-directory-add-domain.md) is the same for the first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="2c9f4-128">Přidat subdomény vlastní domény</span><span class="sxs-lookup"><span data-stu-id="2c9f4-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="2c9f4-129">Pokud chcete přidat název domény třetí úrovně například 'europe.contoso.com' do vašeho adresáře, měli byste nejprve přidat a ověřit domény druhé úrovně, například contoso.com.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-129">If you want to add a third-level domain name such as ‘europe.contoso.com’ to your directory, you should first add and verify the second-level domain, such as contoso.com.</span></span> <span data-ttu-id="2c9f4-130">Subdoméně bude automaticky ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-130">The subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="2c9f4-131">Pokud chcete zjistit, zda byla ověřena subdomény, kterou jste právě přidali, aktualizujte stránku v prohlížeči, který obsahuje seznam domén ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-131">To see that the subdomain that you just added has been verified, refresh the page in the browser that lists the domains in your directory.</span></span>

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="2c9f4-132">Co dělat, když změníte registrátora DNS pro vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="2c9f4-132">What to do if you change the DNS registrar for your custom domain name</span></span>
<span data-ttu-id="2c9f4-133">Pokud změníte registrátora DNS pro vlastní název domény, můžete nadále používat vlastní název domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-133">If you change the DNS registrar for your custom domain name, you can continue to use your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="2c9f4-134">Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci pro tyto služby.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-134">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer to the documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="2c9f4-135">Odstranit vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="2c9f4-135">Delete a custom domain name</span></span>
<span data-ttu-id="2c9f4-136">Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud budete muset použít název domény s jinou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-136">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need to use that domain name with another Azure AD.</span></span>

<span data-ttu-id="2c9f4-137">Chcete-li odstranit vlastní název domény, musíte nejdřív zkontrolovat, že žádné prostředky ve vašem adresáři spoléhají na název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-137">To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name.</span></span> <span data-ttu-id="2c9f4-138">Název domény nelze odstranit z adresáře, pokud:</span><span class="sxs-lookup"><span data-stu-id="2c9f4-138">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="2c9f4-139">Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, která obsahují název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-139">Any user has a user name, email address, or proxy address that include the domain name.</span></span>
* <span data-ttu-id="2c9f4-140">Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-140">Any group has an email address or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="2c9f4-141">Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-141">Any application in your Azure AD has an app ID URI that includes the domain name.</span></span>

<span data-ttu-id="2c9f4-142">Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, chcete-li odstranit vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-142">You must change or delete any such resource in your Azure AD directory before you can delete the custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a><span data-ttu-id="2c9f4-143">Použijte PowerShell nebo rozhraní Graph API, které slouží ke správě názvů domén</span><span class="sxs-lookup"><span data-stu-id="2c9f4-143">Use PowerShell or Graph API to manage domain names</span></span>
<span data-ttu-id="2c9f4-144">Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="2c9f4-144">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="2c9f4-145">Použití Powershellu ke správě názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c9f4-145">Using PowerShell to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="2c9f4-146">Pomocí rozhraní Graph API ke správě názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c9f4-146">Using Graph API to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="2c9f4-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c9f4-147">Next steps</span></span>
* [<span data-ttu-id="2c9f4-148">Další informace o názvech domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c9f4-148">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="2c9f4-149">Správa vlastních názvů domén</span><span class="sxs-lookup"><span data-stu-id="2c9f4-149">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

