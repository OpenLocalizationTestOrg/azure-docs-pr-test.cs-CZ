---
title: "Windows 10 pro podnik: způsoby použití zařízení při práci | Microsoft Docs"
description: "Přehled nasazení zařízení s Windows 10 pro podniky a postup při integraci s Azure Active Directory pro Windows cloudu. Uvádí vedle sebe různé způsoby zařízení můžete zřídit a použít v podniku prostřednictvím portálu Azure."
keywords: "cloudu systému Windows, Windows na Azure Active Directory, zařízení s Windows 10 na Azure, zařízení se systémem Windows Azure"
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2cb9ab6a-55b6-4658-b7f2-6e05ae015e1b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 804156048a7596f9937098e6fe762f424526473c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-10-for-the-enterprise-ways-to-use-devices-for-work"></a><span data-ttu-id="032bc-105">Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="032bc-105">Windows 10 for the enterprise: Ways to use devices for work</span></span>
<span data-ttu-id="032bc-106">Windows 10 vám dává možnost pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="032bc-106">Windows 10 gives you the ability to leverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="032bc-107">Zařízení s Windows 10 můžete připojit ke službě Azure AD, aby uživatelé mohli podepsat systému Windows pomocí účty Azure AD nebo přidáním jejich ID Azure získat přístup k obchodním aplikacím a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="032bc-107">You can connect Windows 10 devices to Azure AD so that users can sign in to Windows by using Azure AD accounts or by adding their Azure IDs to gain access to business apps and resources.</span></span>

![Azure Active Directory s cloudem Windows](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="032bc-109">Integrace zařízení s Windows 10 s Azure Active Directory – mapa obsahu</span><span class="sxs-lookup"><span data-stu-id="032bc-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="032bc-110">V následujících tématech najdete přehled různé možnosti zařízení s Windows 10 ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="032bc-110">The following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="032bc-111">Témata</span><span class="sxs-lookup"><span data-stu-id="032bc-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="032bc-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="032bc-112">Getting started</span></span> |[<span data-ttu-id="032bc-113">Použití zařízení s Windows 10 ve vaší síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="032bc-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="032bc-114">Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="032bc-114">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="032bc-115">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="032bc-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="032bc-116">Nasazení</span><span class="sxs-lookup"><span data-stu-id="032bc-116">Deployment</span></span> |[<span data-ttu-id="032bc-117">Scénáře využití a aspekty nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="032bc-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="032bc-118">Připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne</span><span class="sxs-lookup"><span data-stu-id="032bc-118">Connecting domain-joined devices to Azure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="032bc-119">Povolení Microsoft Passport for work v organizaci</span><span class="sxs-lookup"><span data-stu-id="032bc-119">Enabling Microsoft Passport for work in the organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="032bc-120">Povolení Enterprise State Roaming pro Windows 10</span><span class="sxs-lookup"><span data-stu-id="032bc-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="032bc-121">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="032bc-121">User tasks</span></span> |[<span data-ttu-id="032bc-122">Nastavení nového zařízení Windows 10 s Azure AD během instalace</span><span class="sxs-lookup"><span data-stu-id="032bc-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="032bc-123">Nastavení v nabídce Nastavení Windows 10 zařízení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="032bc-123">Setting up a Windows 10 device with Azure AD from the settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="032bc-124">Připojení osobního zařízení Windows 10 pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="032bc-124">Joining a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md) |

