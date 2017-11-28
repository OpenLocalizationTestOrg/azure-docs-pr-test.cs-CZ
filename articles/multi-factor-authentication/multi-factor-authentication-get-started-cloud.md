---
title: "aaaGet spuštění Azure MFA v cloudu hello | Microsoft Docs"
description: "Toto je stránka hello Microsoft Azure Multi-Factor authentication, která popisuje, jak tooget pracovat s Azure MFA v cloudu hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: kgremban
ms.openlocfilehash: a4aaf44bf08d96f2baad155072fdd6e0e727ce8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-hello-cloud"></a><span data-ttu-id="1291d-103">Začínáme s Azure Multi-Factor Authentication v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="1291d-103">Getting started with Azure Multi-Factor Authentication in hello cloud</span></span>
<span data-ttu-id="1291d-104">Tento článek vás provede způsob spuštění tooget používání ověřování Azure Multi-Factor Authentication v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1291d-104">This article walks through how tooget started using Azure Multi-Factor Authentication in hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="1291d-105">Hello následující dokumentace obsahuje informace o způsobu tooenable uživatele, kteří používají hello **portálu Azure Classic**.</span><span class="sxs-lookup"><span data-stu-id="1291d-105">hello following documentation provides information on how tooenable users using hello **Azure Classic Portal**.</span></span> <span data-ttu-id="1291d-106">Pokud hledáte informace o tom, najdete v části tooset si Azure Multi-Factor Authentication pro uživatele O365, [nastavení služby Multi-Factor authentication pro Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span><span class="sxs-lookup"><span data-stu-id="1291d-106">If you are looking for information on how tooset up Azure Multi-Factor Authentication for O365 users, see [Set up multi-factor authentication for Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)</span></span>

![MFA v cloudu hello](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a><span data-ttu-id="1291d-108">Požadavek</span><span class="sxs-lookup"><span data-stu-id="1291d-108">Prerequisite</span></span>
<span data-ttu-id="1291d-109">[Zaregistrujte si předplatné Azure](https://azure.microsoft.com/pricing/free-trial/) -Pokud už nemáte předplatné Azure, je třeba toosign-up pro jeden.</span><span class="sxs-lookup"><span data-stu-id="1291d-109">[Sign up for an Azure subscription](https://azure.microsoft.com/pricing/free-trial/) - If you do not already have an Azure subscription, you need toosign-up for one.</span></span> <span data-ttu-id="1291d-110">Pokud právě začínáte a používáte Azure MFA, můžete použít zkušební verzi předplatného</span><span class="sxs-lookup"><span data-stu-id="1291d-110">If you are just starting out and using Azure MFA you can use a trial subscription</span></span>

## <a name="enable-azure-multi-factor-authentication"></a><span data-ttu-id="1291d-111">Povolení služby Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1291d-111">Enable Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="1291d-112">Tak dlouho, dokud uživatelé mají licence, které zahrnují Azure Multi-Factor Authentication, není nic, je nutné, aby toodo tooturn na Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="1291d-112">As long as your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="1291d-113">Můžete začít s vyžadováním dvoustupňového ověřování u jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1291d-113">You can start requiring two-step verification on an individual user basis.</span></span> <span data-ttu-id="1291d-114">Hello licence, které umožňují Azure MFA jsou:</span><span class="sxs-lookup"><span data-stu-id="1291d-114">hello licenses that enable Azure MFA are:</span></span>
- <span data-ttu-id="1291d-115">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1291d-115">Azure Multi-Factor Authentication</span></span>
- <span data-ttu-id="1291d-116">Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="1291d-116">Azure Active Directory Premium</span></span>
- <span data-ttu-id="1291d-117">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="1291d-117">Enterprise Mobility + Security</span></span>

<span data-ttu-id="1291d-118">Pokud nemáte jedna z těchto tří licencí, nebo nemáte dostatečný počet licencí toocover všichni uživatelé, které je příliš ok.</span><span class="sxs-lookup"><span data-stu-id="1291d-118">If you don't have one of these three licenses, or you don't have enough licenses toocover all of your users, that's ok too.</span></span> <span data-ttu-id="1291d-119">Právě máte toodo na další krok a [vytvoření poskytovatele Multi-Factor Auth](multi-factor-authentication-get-started-auth-provider.md) ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="1291d-119">You just have toodo an extra step and [Create a Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md) in your directory.</span></span>

## <a name="turn-on-two-step-verification-for-users"></a><span data-ttu-id="1291d-120">Zapnutí dvoustupňového ověřování pro uživatele</span><span class="sxs-lookup"><span data-stu-id="1291d-120">Turn on two-step verification for users</span></span>

<span data-ttu-id="1291d-121">Použijte jeden z postupů hello uvedené v [jak toorequire dvoustupňové ověřování pro uživatele nebo skupinu](multi-factor-authentication-get-started-user-states.md) toostart pomocí Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="1291d-121">Use one of hello procedures listed in [How toorequire two-step verification for a user or group](multi-factor-authentication-get-started-user-states.md) toostart using Azure MFA.</span></span> <span data-ttu-id="1291d-122">Můžete zvolit tooenforce dvoustupňové ověřování pro všechny přihlášení nebo podmíněného přístupu zásady toorequire dvoustupňové ověření můžete vytvořit pouze v případě, že ho záleží tooyou.</span><span class="sxs-lookup"><span data-stu-id="1291d-122">You can choose tooenforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when it matters tooyou.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1291d-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1291d-123">Next Steps</span></span>
<span data-ttu-id="1291d-124">Teď, když jste nastavili Azure Multi-Factor Authentication v cloudu hello, můžete nakonfigurovat a nastavení nasazení.</span><span class="sxs-lookup"><span data-stu-id="1291d-124">Now that you have set up Azure Multi-Factor Authentication in hello cloud, you can configure and set up your deployment.</span></span> <span data-ttu-id="1291d-125">Další podrobnosti najdete v části [Konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="1291d-125">See [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) for more details.</span></span>

