---
title: "Pro vytvoření aplikace Azure AD a nakonfigurujte ho pro přístup k rozhraní API služby Azure Media Services použít 2.0 rozhraní příkazového řádku | Microsoft Docs"
description: "Toto téma ukazuje, jak používat 2.0 rozhraní příkazového řádku k vytvoření aplikace Azure AD a nakonfigurovat ji pro přístup k rozhraní API služby Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 01a2bb6d99776feec936315bc882c3097ce832d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-cli-20-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a><span data-ttu-id="584a0-103">Pomocí rozhraní příkazového řádku 2.0 vytvořit aplikaci AAD a konfigurovat ho pro přístup k rozhraní API služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="584a0-103">Use CLI 2.0 to create an AAD app and configure it to access Azure Media Services API</span></span>

<span data-ttu-id="584a0-104">Toto téma ukazuje, jak vytvořit aplikaci Azure Active Directory (Azure AD) a objektu zabezpečení pro přístup k prostředkům Azure Media Services pomocí rozhraní příkazového řádku 2.0.</span><span class="sxs-lookup"><span data-stu-id="584a0-104">This topic shows you how to use CLI 2.0 to create an Azure Active Directory (Azure AD) application and service principal to access Azure Media Services resources.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="584a0-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="584a0-105">Prerequisites</span></span>

- <span data-ttu-id="584a0-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="584a0-106">An Azure account.</span></span> <span data-ttu-id="584a0-107">Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="584a0-107">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="584a0-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="584a0-108">A Media Services account.</span></span> <span data-ttu-id="584a0-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí webu Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="584a0-109">For more information, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>

## <a name="use-the-azure-cloud-shell"></a><span data-ttu-id="584a0-110">Použití prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="584a0-110">Use the Azure Cloud Shell</span></span>

1. <span data-ttu-id="584a0-111">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="584a0-111">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="584a0-112">Spusťte prostředí cloudu v horním navigačním podokně portálu.</span><span class="sxs-lookup"><span data-stu-id="584a0-112">Launch the Cloud Shell from the upper navigation pane of the portal.</span></span>

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

<span data-ttu-id="584a0-114">Další informace najdete v tématu [Přehled prostředí cloudu Azure](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="584a0-114">For more information, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md).</span></span>

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-cli-20"></a><span data-ttu-id="584a0-115">Vytvoření aplikace Azure AD a konfigurace přístupu k účtu media s 2.0 rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="584a0-115">Create an Azure AD app and configure access to the media account with CLI 2.0</span></span>
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

<span data-ttu-id="584a0-116">Například:</span><span class="sxs-lookup"><span data-stu-id="584a0-116">For example:</span></span>

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

<span data-ttu-id="584a0-117">V tomto příkladu **oboru** je cesta úplné prostředku pro médium účet služby.</span><span class="sxs-lookup"><span data-stu-id="584a0-117">In this example, the **scope** is the full resource path for the media services account.</span></span> <span data-ttu-id="584a0-118">Ale **oboru** může být na všech úrovních.</span><span class="sxs-lookup"><span data-stu-id="584a0-118">However, the **scope** can be at any level.</span></span>

<span data-ttu-id="584a0-119">Například je může použít jeden z následujících úrovní:</span><span class="sxs-lookup"><span data-stu-id="584a0-119">For example, it could be one of the following levels:</span></span>
 
* <span data-ttu-id="584a0-120">**Předplatné** úroveň.</span><span class="sxs-lookup"><span data-stu-id="584a0-120">The **subscription** level.</span></span>
* <span data-ttu-id="584a0-121">**Skupiny prostředků** úroveň.</span><span class="sxs-lookup"><span data-stu-id="584a0-121">The **resource group** level.</span></span>
* <span data-ttu-id="584a0-122">**Prostředků** úrovně (například účet Media).</span><span class="sxs-lookup"><span data-stu-id="584a0-122">The **resource** level (for example, a Media account).</span></span>

<span data-ttu-id="584a0-123">Další informace najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="584a0-123">For more information, see [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

<span data-ttu-id="584a0-124">Viz také [Manage Role-Based řízení přístupu pomocí rozhraní příkazového řádku Azure](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="584a0-124">Also see [Manage Role-Based Access Control with the Azure command-line interface](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="584a0-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="584a0-125">Next steps</span></span>

<span data-ttu-id="584a0-126">Začínáme s [nahrávání souborů do účtu](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="584a0-126">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
