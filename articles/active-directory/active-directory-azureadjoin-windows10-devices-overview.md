---
title: "Windows 10 pro podnik hello: způsoby toouse zařízení pro pracovní | Microsoft Docs"
description: "Přehled nasazení zařízení s Windows 10 pro podniky a jak toointegrate s Azure Active Directory pro hello Windows cloudové. Různé způsoby hello zařízení můžete zřídit a použít v rozlehlé sítě pomocí portálu Azure hello se liší od."
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
ms.openlocfilehash: 95b452bc5ba3937e16de769275a59c77cb821e23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-10-for-hello-enterprise-ways-toouse-devices-for-work"></a><span data-ttu-id="96e12-105">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="96e12-105">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>
<span data-ttu-id="96e12-106">Poskytuje Windows 10 hello tooleverage možnost Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="96e12-106">Windows 10 gives you hello ability tooleverage Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="96e12-107">TooAzure zařízení Windows 10 AD se můžete připojit, aby uživatelé mohli podepsat v tooWindows pomocí účty Azure AD nebo přidáním jejich ID Azure toogain přístup toobusiness aplikacím a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="96e12-107">You can connect Windows 10 devices tooAzure AD so that users can sign in tooWindows by using Azure AD accounts or by adding their Azure IDs toogain access toobusiness apps and resources.</span></span>

![Azure Active Directory s cloudem Windows](./media/active-directory-azureadjoin/windows10-overview.png)

## <a name="integrating-windows-10-devices-with-azure-active-directory--a-content-map"></a><span data-ttu-id="96e12-109">Integrace zařízení s Windows 10 s Azure Active Directory – mapa obsahu</span><span class="sxs-lookup"><span data-stu-id="96e12-109">Integrating Windows 10 devices with Azure Active Directory--a content map</span></span>
<span data-ttu-id="96e12-110">Hello následující témata poskytují přehled o různé možnosti zařízení s Windows 10 ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="96e12-110">hello following topics provide insights into different capabilities of Windows 10 devices in your organization.</span></span>

|  | <span data-ttu-id="96e12-111">Témata</span><span class="sxs-lookup"><span data-stu-id="96e12-111">Topics</span></span> |
| --- | --- |
| <span data-ttu-id="96e12-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="96e12-112">Getting started</span></span> |[<span data-ttu-id="96e12-113">Použití zařízení s Windows 10 ve vaší síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="96e12-113">Using Windows 10 devices in your workplace</span></span>](active-directory-azureadjoin-windows10-devices.md) <br> <br> [<span data-ttu-id="96e12-114">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="96e12-114">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-overview.md) <br> <br> [<span data-ttu-id="96e12-115">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="96e12-115">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md) |
| <span data-ttu-id="96e12-116">Nasazení</span><span class="sxs-lookup"><span data-stu-id="96e12-116">Deployment</span></span> |[<span data-ttu-id="96e12-117">Scénáře využití a aspekty nasazení pro Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="96e12-117">Usage scenarios and deployment considerations for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md) <br><br> [<span data-ttu-id="96e12-118">Připojení zařízení připojených k doméně tooAzure AD pro Windows 10 vyskytne</span><span class="sxs-lookup"><span data-stu-id="96e12-118">Connecting domain-joined devices tooAzure AD, for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)<br><br>[<span data-ttu-id="96e12-119">Povolení Microsoft Passport for work v organizaci hello</span><span class="sxs-lookup"><span data-stu-id="96e12-119">Enabling Microsoft Passport for work in hello organization</span></span>](active-directory-azureadjoin-passport-deployment.md)<br><br> [<span data-ttu-id="96e12-120">Povolení Enterprise State Roaming pro Windows 10</span><span class="sxs-lookup"><span data-stu-id="96e12-120">Enabling Enterprise State Roaming for Windows 10</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)<br><br> |
| <span data-ttu-id="96e12-121">Úlohy uživatele</span><span class="sxs-lookup"><span data-stu-id="96e12-121">User tasks</span></span> |[<span data-ttu-id="96e12-122">Nastavení nového zařízení Windows 10 s Azure AD během instalace</span><span class="sxs-lookup"><span data-stu-id="96e12-122">Setting up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md) <br><br> [<span data-ttu-id="96e12-123">Nastavení v nabídce nastavení hello zařízením s Windows 10 s Azure AD</span><span class="sxs-lookup"><span data-stu-id="96e12-123">Setting up a Windows 10 device with Azure AD from hello settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md) <br><br> [<span data-ttu-id="96e12-124">Propojení na osobní organizaci tooyour zařízení Windows 10</span><span class="sxs-lookup"><span data-stu-id="96e12-124">Joining a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md) |

