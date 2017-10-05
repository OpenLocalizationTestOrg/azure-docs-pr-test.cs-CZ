---
title: "Rozšíření aplikace – Azure AD B2C | Microsoft Docs"
description: "Obnovení aplikace rozšíření b2c"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: 17500b572a0e92c1c233c6967840a5b6d96e21cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-extensions-app"></a><span data-ttu-id="8f0ff-103">Azure AD B2C: Rozšíření aplikace</span><span class="sxs-lookup"><span data-stu-id="8f0ff-103">Azure AD B2C: Extensions app</span></span>

<span data-ttu-id="8f0ff-104">Když je vytvořen adresář služby Azure AD B2C, aplikace volá `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` se automaticky vytvoří do nového adresáře.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-104">When an Azure AD B2C directory is created, an app called `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` is automatically created inside the new directory.</span></span> <span data-ttu-id="8f0ff-105">Tuto aplikaci, označuje jako **aplikace b2c rozšíření**, se zobrazí na *registrace aplikace*.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-105">This app, referred to as the **b2c-extensions-app**, is visible in *App registrations*.</span></span> <span data-ttu-id="8f0ff-106">Pomocí služby Azure AD B2C se používá k ukládání informací o uživatelích a vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-106">It is used by the Azure AD B2C service to store information about users and custom attributes.</span></span> <span data-ttu-id="8f0ff-107">Pokud se odstraní aplikaci, Azure AD B2C nebude fungovat správně a bude mít vliv na produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-107">If the app is deleted, Azure AD B2C will not function correctly and your production environment will be affected.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f0ff-108">Pokud máte v úmyslu okamžitě odstranit váš klient, neodstraňujte aplikace rozšíření b2c.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-108">Do not delete the b2c-extensions-app unless you are planning to immediately delete your tenant.</span></span> <span data-ttu-id="8f0ff-109">Pokud aplikace zůstane odstraněné déle než 30 dní, uživatel informace budou trvale ztratí.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-109">If the app remains deleted for more than 30 days, user information will be permanently lost.</span></span>

## <a name="verifying-that-the-extensions-app-is-present"></a><span data-ttu-id="8f0ff-110">Ověření, zda je k dispozici rozšíření aplikace</span><span class="sxs-lookup"><span data-stu-id="8f0ff-110">Verifying that the extensions app is present</span></span>

<span data-ttu-id="8f0ff-111">Chcete-li ověřit, zda aplikace rozšíření b2c je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="8f0ff-111">To verify that the b2c-extensions-app is present:</span></span>

1. <span data-ttu-id="8f0ff-112">Ve vašem klientu Azure AD B2C, klikněte na **další služby** v levé navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-112">Inside your Azure AD B2C tenant, click on **More services** in the left-hand navigation menu.</span></span>
1. <span data-ttu-id="8f0ff-113">Vyhledejte a otevřete **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-113">Search for and open **App registrations**.</span></span>
1. <span data-ttu-id="8f0ff-114">Vyhledejte aplikaci, která začíná **aplikace b2c rozšíření**</span><span class="sxs-lookup"><span data-stu-id="8f0ff-114">Look for an app that begins with **b2c-extensions-app**</span></span>

## <a name="recover-the-extensions-app"></a><span data-ttu-id="8f0ff-115">Obnovit rozšíření aplikace</span><span class="sxs-lookup"><span data-stu-id="8f0ff-115">Recover the extensions app</span></span>

<span data-ttu-id="8f0ff-116">Pokud jste omylem odstranili aplikace rozšíření b2c, máte 30 dní, jak jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-116">If you accidentally deleted the b2c-extensions-app, you have 30 days to recover it.</span></span> <span data-ttu-id="8f0ff-117">Můžete obnovit aplikace pomocí rozhraní Graph API:</span><span class="sxs-lookup"><span data-stu-id="8f0ff-117">You can restore the app using the Graph API:</span></span>

1. <span data-ttu-id="8f0ff-118">Přejděte do [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="8f0ff-118">Browse to [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span>
1. <span data-ttu-id="8f0ff-119">Přihlaste se k webu jako globální správce adresáře Azure AD B2C, který chcete obnovit odstraněné aplikace pro.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-119">Log in to the site as a global administrator for the Azure AD B2C directory that you want to restore the deleted app for.</span></span>
1. <span data-ttu-id="8f0ff-120">Vydávat na HTTP GET na adresu URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` s verze api-version = 1.6.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-120">Issue an HTTP GET against the URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` with api-version=1.6.</span></span> <span data-ttu-id="8f0ff-121">Nezapomeňte nahradit `{tenantName}` názvem svého klienta.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-121">Make sure to replace `{tenantName}` with your tenant name.</span></span> <span data-ttu-id="8f0ff-122">Tato operace se zobrazí seznam všech aplikacích, které byly odstraněny v posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-122">This operation will list all of the applications that have been deleted within the past 30 days.</span></span>
1. <span data-ttu-id="8f0ff-123">Vyhledat aplikaci v seznamu, kde název začíná 'aplikace b2c rozšíření a zkopírujte její `objectid` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-123">Find the application in the list where the name begins with 'b2c-extension-app’ and copy its `objectid` property value.</span></span>
1. <span data-ttu-id="8f0ff-124">Vydání požadavku HTTP POST na adresu URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-124">Issue an HTTP POST against the URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span></span> <span data-ttu-id="8f0ff-125">Nahraďte `{OBJECTID}` v adrese URL se `objectid` z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-125">Replace the `{OBJECTID}` portion of the URL with the `objectid` from the previous step.</span></span> 

<span data-ttu-id="8f0ff-126">Nyní byste měli být schopni [obnovené aplikace](#verifying-that-the-extensions-app-is-present) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-126">You should now be able to [see the restored app](#verifying-that-the-extensions-app-is-present) in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="8f0ff-127">Aplikace lze obnovit pouze pokud byl odstraněn během posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-127">An application can only be restored if it has been deleted within the last 30 days.</span></span> <span data-ttu-id="8f0ff-128">Pokud to bylo víc než 30 dní, data budou trvale ztratí.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-128">If it has been more than 30 days, data will be permanently lost.</span></span> <span data-ttu-id="8f0ff-129">O další pomoc soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="8f0ff-129">For more assistance, file a support ticket.</span></span>