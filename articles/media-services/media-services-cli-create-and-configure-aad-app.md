---
title: "toocreate aaaUse 2.0 rozhraní příkazového řádku aplikace Azure AD a nakonfigurujte ji tooaccess rozhraní API služby Azure Media Services | Microsoft Docs"
description: "Toto téma ukazuje, jak toocreate toouse 2.0 rozhraní příkazového řádku aplikace Azure AD a nakonfigurujte ji tooaccess rozhraní API služby Azure Media Services."
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
ms.openlocfilehash: c865e2701722374b5dd17b0e20fa848c07065006
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a><span data-ttu-id="f496b-103">Použijte rozhraní příkazového řádku 2.0 toocreate AAD aplikace a nakonfigurujte ho tooaccess rozhraní API služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="f496b-103">Use CLI 2.0 toocreate an AAD app and configure it tooaccess Azure Media Services API</span></span>

<span data-ttu-id="f496b-104">Toto téma ukazuje, jak toouse 2.0 rozhraní příkazového řádku toocreate aplikaci Azure Active Directory (Azure AD) a hlavní tooaccess služby Azure Media Services prostředky.</span><span class="sxs-lookup"><span data-stu-id="f496b-104">This topic shows you how toouse CLI 2.0 toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f496b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f496b-105">Prerequisites</span></span>

- <span data-ttu-id="f496b-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f496b-106">An Azure account.</span></span> <span data-ttu-id="f496b-107">Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f496b-107">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="f496b-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="f496b-108">A Media Services account.</span></span> <span data-ttu-id="f496b-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="f496b-109">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>

## <a name="use-hello-azure-cloud-shell"></a><span data-ttu-id="f496b-110">Použití hello prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="f496b-110">Use hello Azure Cloud Shell</span></span>

1. <span data-ttu-id="f496b-111">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f496b-111">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f496b-112">Spusťte hello cloudové prostředí v horním navigačním podokně hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="f496b-112">Launch hello Cloud Shell from hello upper navigation pane of hello portal.</span></span>

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

<span data-ttu-id="f496b-114">Další informace najdete v tématu [Přehled prostředí cloudu Azure](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="f496b-114">For more information, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md).</span></span>

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a><span data-ttu-id="f496b-115">Vytvoření aplikace Azure AD a nakonfigurovat účet přístupu k toohello média pomocí rozhraní příkazového řádku 2.0</span><span class="sxs-lookup"><span data-stu-id="f496b-115">Create an Azure AD app and configure access toohello media account with CLI 2.0</span></span>
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

<span data-ttu-id="f496b-116">Například:</span><span class="sxs-lookup"><span data-stu-id="f496b-116">For example:</span></span>

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

<span data-ttu-id="f496b-117">V tomto příkladu hello **oboru** je cesta hello úplné prostředků pro média hello účtu služby.</span><span class="sxs-lookup"><span data-stu-id="f496b-117">In this example, hello **scope** is hello full resource path for hello media services account.</span></span> <span data-ttu-id="f496b-118">Ale hello **oboru** může být na všech úrovních.</span><span class="sxs-lookup"><span data-stu-id="f496b-118">However, hello **scope** can be at any level.</span></span>

<span data-ttu-id="f496b-119">Například je může použít jeden z následujících úrovní hello:</span><span class="sxs-lookup"><span data-stu-id="f496b-119">For example, it could be one of hello following levels:</span></span>
 
* <span data-ttu-id="f496b-120">Hello **předplatné** úroveň.</span><span class="sxs-lookup"><span data-stu-id="f496b-120">hello **subscription** level.</span></span>
* <span data-ttu-id="f496b-121">Hello **skupiny prostředků** úroveň.</span><span class="sxs-lookup"><span data-stu-id="f496b-121">hello **resource group** level.</span></span>
* <span data-ttu-id="f496b-122">Hello **prostředků** úrovně (například účet Media).</span><span class="sxs-lookup"><span data-stu-id="f496b-122">hello **resource** level (for example, a Media account).</span></span>

<span data-ttu-id="f496b-123">Další informace najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="f496b-123">For more information, see [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

<span data-ttu-id="f496b-124">Viz také [Manage Role-Based řízení přístupu pomocí rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f496b-124">Also see [Manage Role-Based Access Control with hello Azure command-line interface](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f496b-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f496b-125">Next steps</span></span>

<span data-ttu-id="f496b-126">Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="f496b-126">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
