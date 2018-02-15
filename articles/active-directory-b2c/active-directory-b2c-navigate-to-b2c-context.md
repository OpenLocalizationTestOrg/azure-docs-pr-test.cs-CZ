---
title: "Azure Active Directory B2C: Přepnutí na tenanta B2C | Dokumentace Microsoftu"
description: "Postup přepnutí do kontextu tenanta Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 6c1fd08c52f33a062d06e0593cbbe00346bb44f1
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a><span data-ttu-id="dd450-103">Přepnutí na tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="dd450-103">Switching to your Azure AD B2C tenant</span></span>

<span data-ttu-id="dd450-104">Abyste mohli konfigurovat Azure AD B2C, musíte být v kontextu tenanta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-104">In order to configure Azure AD B2C, you need to be in the context of your Azure AD B2C tenant.</span></span>

## <a name="log-into-azure-ad-b2c-tenant"></a><span data-ttu-id="dd450-105">Přihlášení k tenantovi Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="dd450-105">Log into Azure AD B2C tenant</span></span>

<span data-ttu-id="dd450-106">Abyste mohli přejít do tenanta Azure AD B2C, musíte být přihlášeni na webu Azure Portal jako globální správce tenanta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-106">To navigate to your Azure AD B2C tenant, you must be logged into the Azure portal as a global administrator of the Azure AD B2C tenant.</span></span>

1. <span data-ttu-id="dd450-107">Přihlaste se k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd450-107">Sign into the [Azure portal](http://portal.azure.com).</span></span>
1. <span data-ttu-id="dd450-108">Tenanty můžete přepínat kliknutím na vaši e-mailovou adresu nebo váš obrázek v horním pravém rohu.</span><span class="sxs-lookup"><span data-stu-id="dd450-108">Switch tenants by clicking your email address or picture in the top-right corner.</span></span>
1. <span data-ttu-id="dd450-109">V seznamu `Directory`, který se zobrazí, vyberte tenanta Azure AD B2C, kterého chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="dd450-109">In the `Directory` list that appears, select the Azure AD B2C tenant that you wish to manage.</span></span>

<span data-ttu-id="dd450-110">Azure Portal se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="dd450-110">The Azure Portal will refresh.</span></span>  <span data-ttu-id="dd450-111">Nyní jste na webu Azure Portal přihlášeni v kontextu vašeho tenanta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-111">Now you are signed into the Azure Portal in the context of your Azure AD B2C tenant.</span></span>

## <a name="navigate-to-the-b2c-features-blade"></a><span data-ttu-id="dd450-112">Přejděte do okna s funkcemi B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-112">Navigate to the B2C features blade</span></span>

1. <span data-ttu-id="dd450-113">Klikněte na **Procházet** v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="dd450-113">Click **Browse** on the left hand navigation.</span></span>
1. <span data-ttu-id="dd450-114">Klikněte na **> Další služby** a pak v levém navigačním panelu vyhledejte `Azure AD B2C`.</span><span class="sxs-lookup"><span data-stu-id="dd450-114">Click **> More services** and then search for `Azure AD B2C` in the left navigation pane.</span></span>  <span data-ttu-id="dd450-115">(Pro připnutí na Úvodní panel vlevo klikněte na hvězdičku nalevo od Azure AD B2C)</span><span class="sxs-lookup"><span data-stu-id="dd450-115">(To pin to your left-hand Startboard, click the star to the left of Azure AD B2C)</span></span>
1. <span data-ttu-id="dd450-116">Kliknutím na **Azure AD B2C** otevřete okno s funkcemi B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-116">Click **Azure AD B2C** to access the B2C features blade.</span></span>
   
    ![Snímek obrazovky s přechodem do okna s funkcemi B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> <span data-ttu-id="dd450-118">Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="dd450-118">You need to be a Global Administrator of the B2C tenant to be able to access the B2C features blade.</span></span> <span data-ttu-id="dd450-119">Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.</span><span class="sxs-lookup"><span data-stu-id="dd450-119">A Global Administrator from any other tenant or a user from any tenant cannot access it.</span></span>  <span data-ttu-id="dd450-120">Můžete přepnout na svého klienta B2C pomocí přepínače klienta v pravém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dd450-120">You can switch to your B2C tenant by using the tenant switcher in the top right corner of the Azure portal.</span></span>
