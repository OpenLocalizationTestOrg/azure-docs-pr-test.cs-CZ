---
title: "aaaJoin v organizaci tooyour osobní zařízení | Microsoft Docs"
description: "Vysvětluje, jak uživatelé mohou registrovat své osobní Windows 10 zařízení tootheir podnikové síti a poskytuje kroky nasazení pro scénáři modelu BYOD."
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
ms.openlocfilehash: 979e2461dd9ad0438aa402a0a3223cc84c4c625c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-personal-device-tooyour-organization"></a><span data-ttu-id="b2a3f-103">Připojení organizaci tooyour osobní zařízení</span><span class="sxs-lookup"><span data-stu-id="b2a3f-103">Join a personal device tooyour organization</span></span>
## <a name="toojoin-a-windows-10-device-tooyour-organization"></a><span data-ttu-id="b2a3f-104">organizace tooyour zařízení toojoin Windows 10</span><span class="sxs-lookup"><span data-stu-id="b2a3f-104">toojoin a Windows 10 device tooyour organization</span></span>
1. <span data-ttu-id="b2a3f-105">Z hello **spustit** nabídce vyberte možnost **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-105">From hello **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="b2a3f-106">Vyberte **účty**a potom klikněte na **účtu**.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="b2a3f-107">Klikněte na tlačítko **přidat pracovní nebo školní účet**a poté zadejte v účtu organizace.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="b2a3f-108">Na hello přihlašovací stránku vaší organizace, zadejte uživatelské jméno a heslo a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-108">On hello sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="b2a3f-109">Zobrazí se výzva pro výzvu ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="b2a3f-110">(Tento problém lze konfigurovat správcem IT).</span><span class="sxs-lookup"><span data-stu-id="b2a3f-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="b2a3f-111">Azure Active Directory (Azure AD) kontroluje, zda text hello zařízení vyžaduje registraci správy mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-111">Azure Active Directory (Azure AD) checks whether hello device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="b2a3f-112">Windows hello zařízení registruje v adresáři organizace hello ve službě Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-112">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="b2a3f-113">Pokud jste spravované uživatele, trvá Windows desktop toohello prostřednictvím hello automatické přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-113">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in.</span></span>
9. <span data-ttu-id="b2a3f-114">Pokud se Federovaný uživatel, bude provedena tooa přihlášení Windows obrazovky tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b2a3f-114">If you are a federated user, you will be taken tooa Windows sign-in screen tooenter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="b2a3f-115">Další informace</span><span class="sxs-lookup"><span data-stu-id="b2a3f-115">Additional information</span></span>
* [<span data-ttu-id="b2a3f-116">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="b2a3f-116">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="b2a3f-117">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="b2a3f-117">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="b2a3f-118">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="b2a3f-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="b2a3f-119">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="b2a3f-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="b2a3f-120">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="b2a3f-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="b2a3f-121">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="b2a3f-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

