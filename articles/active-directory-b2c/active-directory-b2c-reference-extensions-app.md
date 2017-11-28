---
title: "Rozšíření aplikace – Azure AD B2C | Microsoft Docs"
description: "Obnovení hello b2c-rozšíření – aplikace"
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
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a><span data-ttu-id="866c8-103">Azure AD B2C: Rozšíření aplikace</span><span class="sxs-lookup"><span data-stu-id="866c8-103">Azure AD B2C: Extensions app</span></span>

<span data-ttu-id="866c8-104">Když je vytvořen adresář služby Azure AD B2C, aplikace volá `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` se automaticky vytvoří uvnitř hello nový adresář.</span><span class="sxs-lookup"><span data-stu-id="866c8-104">When an Azure AD B2C directory is created, an app called `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` is automatically created inside hello new directory.</span></span> <span data-ttu-id="866c8-105">Pro tuto aplikaci, označují tooas hello **aplikace b2c rozšíření**, se zobrazí na *registrace aplikace*.</span><span class="sxs-lookup"><span data-stu-id="866c8-105">This app, referred tooas hello **b2c-extensions-app**, is visible in *App registrations*.</span></span> <span data-ttu-id="866c8-106">Je používána hello služby Azure AD B2C toostore. informace o uživatelích a vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="866c8-106">It is used by hello Azure AD B2C service toostore information about users and custom attributes.</span></span> <span data-ttu-id="866c8-107">Pokud aplikace hello odstraněna, Azure AD B2C nebude fungovat správně a bude mít vliv na produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="866c8-107">If hello app is deleted, Azure AD B2C will not function correctly and your production environment will be affected.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="866c8-108">Neodstraňujte hello b2c-rozšíření – aplikace, pokud plánujete tooimmediately odstranění vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="866c8-108">Do not delete hello b2c-extensions-app unless you are planning tooimmediately delete your tenant.</span></span> <span data-ttu-id="866c8-109">Pokud aplikace hello zůstane odstraněné déle než 30 dní, uživatel informace budou trvale ztratí.</span><span class="sxs-lookup"><span data-stu-id="866c8-109">If hello app remains deleted for more than 30 days, user information will be permanently lost.</span></span>

## <a name="verifying-that-hello-extensions-app-is-present"></a><span data-ttu-id="866c8-110">Ověření rozšíření aplikaci hello je k dispozici</span><span class="sxs-lookup"><span data-stu-id="866c8-110">Verifying that hello extensions app is present</span></span>

<span data-ttu-id="866c8-111">tooverify, který hello aplikace b2c rozšíření je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="866c8-111">tooverify that hello b2c-extensions-app is present:</span></span>

1. <span data-ttu-id="866c8-112">Ve vašem klientu Azure AD B2C, klikněte na **další služby** v levé navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="866c8-112">Inside your Azure AD B2C tenant, click on **More services** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="866c8-113">Vyhledejte a otevřete **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="866c8-113">Search for and open **App registrations**.</span></span>
1. <span data-ttu-id="866c8-114">Vyhledejte aplikaci, která začíná **aplikace b2c rozšíření**</span><span class="sxs-lookup"><span data-stu-id="866c8-114">Look for an app that begins with **b2c-extensions-app**</span></span>

## <a name="recover-hello-extensions-app"></a><span data-ttu-id="866c8-115">Obnovit hello rozšíření aplikace</span><span class="sxs-lookup"><span data-stu-id="866c8-115">Recover hello extensions app</span></span>

<span data-ttu-id="866c8-116">Pokud jste omylem odstranili hello b2c-rozšíření – aplikace, máte toorecover 30 dní ho.</span><span class="sxs-lookup"><span data-stu-id="866c8-116">If you accidentally deleted hello b2c-extensions-app, you have 30 days toorecover it.</span></span> <span data-ttu-id="866c8-117">Můžete obnovit pomocí hello rozhraní Graph API aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="866c8-117">You can restore hello app using hello Graph API:</span></span>

1. <span data-ttu-id="866c8-118">Procházet příliš[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="866c8-118">Browse too[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).</span></span>
1. <span data-ttu-id="866c8-119">Přihlaste se v lokalitě toohello jako globálního správce adresáře Azure AD B2C hello, které chcete aplikaci odstranit hello toorestore pro.</span><span class="sxs-lookup"><span data-stu-id="866c8-119">Log in toohello site as a global administrator for hello Azure AD B2C directory that you want toorestore hello deleted app for.</span></span>
1. <span data-ttu-id="866c8-120">Vystavení HTTP GET na adresu URL hello `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` s verze api-version = 1.6.</span><span class="sxs-lookup"><span data-stu-id="866c8-120">Issue an HTTP GET against hello URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` with api-version=1.6.</span></span> <span data-ttu-id="866c8-121">Ujistěte se, že tooreplace `{tenantName}` s název vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="866c8-121">Make sure tooreplace `{tenantName}` with your tenant name.</span></span> <span data-ttu-id="866c8-122">Tato operace se zobrazí seznam všech hello aplikací, které byly odstraněny v rámci hello posledních 30 dnech.</span><span class="sxs-lookup"><span data-stu-id="866c8-122">This operation will list all of hello applications that have been deleted within hello past 30 days.</span></span>
1. <span data-ttu-id="866c8-123">Najít hello aplikace v seznamu hello, kde název hello začíná "aplikace b2c rozšíření a zkopírujte její `objectid` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="866c8-123">Find hello application in hello list where hello name begins with 'b2c-extension-app’ and copy its `objectid` property value.</span></span>
1. <span data-ttu-id="866c8-124">Vydání požadavku HTTP POST na adresu URL hello `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span><span class="sxs-lookup"><span data-stu-id="866c8-124">Issue an HTTP POST against hello URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`.</span></span> <span data-ttu-id="866c8-125">Nahraďte hello `{OBJECTID}` část adresy URL hello s hello `objectid` hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="866c8-125">Replace hello `{OBJECTID}` portion of hello URL with hello `objectid` from hello previous step.</span></span> 

<span data-ttu-id="866c8-126">Teď byste měli být schopný příliš[aplikace hello obnovit](#verifying-that-the-extensions-app-is-present) v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="866c8-126">You should now be able too[see hello restored app](#verifying-that-the-extensions-app-is-present) in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="866c8-127">Aplikace lze obnovit pouze pokud byla odstraněna v rámci hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="866c8-127">An application can only be restored if it has been deleted within hello last 30 days.</span></span> <span data-ttu-id="866c8-128">Pokud to bylo víc než 30 dní, data budou trvale ztratí.</span><span class="sxs-lookup"><span data-stu-id="866c8-128">If it has been more than 30 days, data will be permanently lost.</span></span> <span data-ttu-id="866c8-129">O další pomoc soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="866c8-129">For more assistance, file a support ticket.</span></span>
