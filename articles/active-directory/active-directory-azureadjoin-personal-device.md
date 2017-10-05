---
title: "Připojení osobního zařízení pro vaši organizaci | Microsoft Docs"
description: "Vysvětluje, jak uživatelé mohou registrovat svá osobní zařízení Windows 10 do své podnikové síti a poskytuje kroky nasazení pro scénáři modelu BYOD."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a><span data-ttu-id="b3aac-103">Připojení osobního zařízení pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="b3aac-103">Join a personal device to your organization</span></span>
## <a name="to-join-a-windows-10-device-to-your-organization"></a><span data-ttu-id="b3aac-104">K připojení k zařízením s Windows 10 pro vaši organizaci</span><span class="sxs-lookup"><span data-stu-id="b3aac-104">To join a Windows 10 device to your organization</span></span>
1. <span data-ttu-id="b3aac-105">Z **spustit** nabídce vyberte možnost **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b3aac-105">From the **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="b3aac-106">Vyberte **účty**a potom klikněte na **účtu**.</span><span class="sxs-lookup"><span data-stu-id="b3aac-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="b3aac-107">Klikněte na tlačítko **přidat pracovní nebo školní účet**a poté zadejte v účtu organizace.</span><span class="sxs-lookup"><span data-stu-id="b3aac-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="b3aac-108">Na přihlašovací stránce pro vaši organizaci, zadejte uživatelské jméno a heslo a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3aac-108">On the sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="b3aac-109">Zobrazí se výzva pro výzvu ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="b3aac-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="b3aac-110">(Tento problém lze konfigurovat správcem IT).</span><span class="sxs-lookup"><span data-stu-id="b3aac-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="b3aac-111">Azure Active Directory (Azure AD) zkontroluje, jestli zařízení vyžaduje registraci správy mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3aac-111">Azure Active Directory (Azure AD) checks whether the device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="b3aac-112">Systém Windows se zařízení registruje do adresáře organizace v Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b3aac-112">Windows registers the device in the organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="b3aac-113">Pokud jste spravované uživatel, Windows přejdete na ploše prostřednictvím automatického přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b3aac-113">If you are a managed user, Windows takes you to the desktop through the automatic sign-in.</span></span>
9. <span data-ttu-id="b3aac-114">Pokud se Federovaný uživatel, můžete k zadání pověření přesměrováni na obrazovce přihlášení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b3aac-114">If you are a federated user, you will be taken to a Windows sign-in screen to enter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="b3aac-115">Další informace</span><span class="sxs-lookup"><span data-stu-id="b3aac-115">Additional information</span></span>
* [<span data-ttu-id="b3aac-116">Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="b3aac-116">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="b3aac-117">Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="b3aac-117">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="b3aac-118">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="b3aac-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="b3aac-119">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="b3aac-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="b3aac-120">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="b3aac-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="b3aac-121">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="b3aac-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

