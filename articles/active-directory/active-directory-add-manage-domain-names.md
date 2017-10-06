---
title: "aaaManaging vlastních názvů domén v Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="acce8-103">Správa vlastních názvů domén v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="acce8-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="acce8-104">Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:</span><span class="sxs-lookup"><span data-stu-id="acce8-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="acce8-105">Pro uživatele uživatelského jména nebo e-mailové adresy</span><span class="sxs-lookup"><span data-stu-id="acce8-105">A user name or email address for a user</span></span>
* <span data-ttu-id="acce8-106">Hello adresy pro skupinu</span><span class="sxs-lookup"><span data-stu-id="acce8-106">hello address for a group</span></span>
* <span data-ttu-id="acce8-107">identifikátor ID URI aplikace Hello pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="acce8-107">hello app ID URI for an application</span></span>

<span data-ttu-id="acce8-108">Název domény, který je již ověřit, že toobe vlastníkem hello adresáře, která obsahuje hello prostředků může obsahovat prostředků v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="acce8-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified toobe owned by hello directory that contains hello resource.</span></span> <span data-ttu-id="acce8-109">Globální správce můžete provádět úlohy správy domény ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acce8-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="acce8-110">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="acce8-110">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="acce8-111">Jak toomanage vaše názvů domén v Centru pro správu hello Azure AD, najdete v části [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="acce8-111">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="acce8-112">Nastavte název hello primární domény pro váš adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="acce8-112">Set hello primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="acce8-113">Při vytváření adresáře hello počáteční název domény, jako je například 'contoso.onmicrosoft.com', je také název hello primární domény pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="acce8-113">When your directory is created, hello initial domain name, such as ‘contoso.onmicrosoft.com,’ is also hello primary domain name for your directory.</span></span> <span data-ttu-id="acce8-114">primární doménu Hello je hello výchozí název domény pro nového uživatele při vytváření nového uživatele v hello [portál Azure classic](https://manage.windowsazure.com/), nebo dalších portálech, jako je například portál pro správu hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="acce8-114">hello primary domain is hello default domain name for a new user when you create a new user in hello [Azure classic portal](https://manage.windowsazure.com/), or other portals such as hello Office 365 admin portal.</span></span> <span data-ttu-id="acce8-115">To zjednodušuje proces hello správce toocreate noví uživatelé portálu hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-115">This streamlines hello process for an administrator toocreate new users in hello portal.</span></span>

<span data-ttu-id="acce8-116">Název toochange hello primární domény pro váš adresář:</span><span class="sxs-lookup"><span data-stu-id="acce8-116">toochange hello primary domain name for your directory:</span></span>

1. <span data-ttu-id="acce8-117">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acce8-117">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="acce8-118">Vyberte **služby Active Directory** na levém navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-118">Select **Active Directory** on hello left navigation bar.</span></span>
3. <span data-ttu-id="acce8-119">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="acce8-119">Open your directory.</span></span>
4. <span data-ttu-id="acce8-120">Vyberte hello **domén** kartě.</span><span class="sxs-lookup"><span data-stu-id="acce8-120">Select hello **Domains** tab.</span></span>
5. <span data-ttu-id="acce8-121">Vyberte hello **změnu primární** tlačítka na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-121">Select hello **Change primary** button on hello command bar.</span></span>
6. <span data-ttu-id="acce8-122">Vyberte doménu hello chcete toobe hello nové primární domény pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="acce8-122">Select hello domain that you want toobe hello new primary domain for your directory.</span></span>

<span data-ttu-id="acce8-123">Můžete změnit název hello primární domény pro váš adresář toobe všechny ověřené vlastní doménu, která není federovaný.</span><span class="sxs-lookup"><span data-stu-id="acce8-123">You can change hello primary domain name for your directory toobe any verified custom domain that is not federated.</span></span> <span data-ttu-id="acce8-124">Změna hello primární domény pro váš adresář nedojde ke změně hello uživatelská jména pro všechny stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="acce8-124">Changing hello primary domain for your directory will not change hello user names for any existing users.</span></span>

## <a name="add-custom-domain-names-tooyour-azure-ad"></a><span data-ttu-id="acce8-125">Přidat vlastní doménu názvy tooyour Azure AD</span><span class="sxs-lookup"><span data-stu-id="acce8-125">Add custom domain names tooyour Azure AD</span></span>
<span data-ttu-id="acce8-126">Můžete přidat až adresář Azure AD tooeach názvy vlastních domén too900.</span><span class="sxs-lookup"><span data-stu-id="acce8-126">You can add up too900 custom domain names tooeach Azure AD directory.</span></span> <span data-ttu-id="acce8-127">Hello proces příliš[, přidejte další vlastní domény název](active-directory-add-domain.md) hello stejné pro hello první vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="acce8-127">hello process too[add an additional custom domain name](active-directory-add-domain.md) is hello same for hello first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="acce8-128">Přidat subdomény vlastní domény</span><span class="sxs-lookup"><span data-stu-id="acce8-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="acce8-129">Pokud chcete tooadd názvu domény třetí úrovně, třeba directory tooyour 'europe.contoso.com', měli byste nejprve přidat a ověřit hello domény druhé úrovně, jako je například contoso.com. Hello subdomény bude automaticky ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acce8-129">If you want tooadd a third-level domain name such as ‘europe.contoso.com’ tooyour directory, you should first add and verify hello second-level domain, such as contoso.com. hello subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="acce8-130">ověření, toosee, který hello subdomény, kterou jste právě přidali, aktualizace hello stránku hello prohlížeč, který obsahuje seznam domén hello ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="acce8-130">toosee that hello subdomain that you just added has been verified, refresh hello page in hello browser that lists hello domains in your directory.</span></span>

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="acce8-131">Jaké toodo, pokud změníte hello registrátora DNS pro vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="acce8-131">What toodo if you change hello DNS registrar for your custom domain name</span></span>
<span data-ttu-id="acce8-132">Pokud změníte hello registrátora DNS pro vlastní název domény, můžete dál toouse vlastního názvu domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy.</span><span class="sxs-lookup"><span data-stu-id="acce8-132">If you change hello DNS registrar for your custom domain name, you can continue toouse your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="acce8-133">Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci toohello pro tyto služby.</span><span class="sxs-lookup"><span data-stu-id="acce8-133">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer toohello documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="acce8-134">Odstranit vlastní název domény</span><span class="sxs-lookup"><span data-stu-id="acce8-134">Delete a custom domain name</span></span>
<span data-ttu-id="acce8-135">Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud potřebujete toouse název domény s jinou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acce8-135">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need toouse that domain name with another Azure AD.</span></span>

<span data-ttu-id="acce8-136">toodelete vlastní název domény, musíte napřed zajistit, aby žádné prostředky ve vašem adresáři spoléhají na název domény hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-136">toodelete a custom domain name, you must first ensure that no resources in your directory rely on hello domain name.</span></span> <span data-ttu-id="acce8-137">Název domény nelze odstranit z adresáře, pokud:</span><span class="sxs-lookup"><span data-stu-id="acce8-137">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="acce8-138">Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, která obsahují název domény hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-138">Any user has a user name, email address, or proxy address that include hello domain name.</span></span>
* <span data-ttu-id="acce8-139">Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, která zahrnuje název domény hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-139">Any group has an email address or proxy address that includes hello domain name.</span></span>
* <span data-ttu-id="acce8-140">Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény hello.</span><span class="sxs-lookup"><span data-stu-id="acce8-140">Any application in your Azure AD has an app ID URI that includes hello domain name.</span></span>

<span data-ttu-id="acce8-141">Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, než budete moct odstranit hello vlastní název domény.</span><span class="sxs-lookup"><span data-stu-id="acce8-141">You must change or delete any such resource in your Azure AD directory before you can delete hello custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a><span data-ttu-id="acce8-142">Pomocí prostředí PowerShell nebo rozhraní Graph API názvů domén toomanage</span><span class="sxs-lookup"><span data-stu-id="acce8-142">Use PowerShell or Graph API toomanage domain names</span></span>
<span data-ttu-id="acce8-143">Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="acce8-143">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="acce8-144">Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="acce8-144">Using PowerShell toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="acce8-145">Pomocí názvů domén toomanage rozhraní Graph API v Azure AD</span><span class="sxs-lookup"><span data-stu-id="acce8-145">Using Graph API toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="acce8-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acce8-146">Next steps</span></span>
* [<span data-ttu-id="acce8-147">Další informace o názvech domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="acce8-147">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="acce8-148">Správa vlastních názvů domén</span><span class="sxs-lookup"><span data-stu-id="acce8-148">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

