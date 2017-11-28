---
title: "aaaSet se zařízením s Windows 10 s Azure AD z nastavení | Microsoft Docs"
description: "Vysvětluje, jak uživatelé se můžete zapojit do tooAzure AD pomocí nabídky nastavení hello."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="896d5-103">Nastavení Windows 10 zařízení s Azure AD z nastavení</span><span class="sxs-lookup"><span data-stu-id="896d5-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="896d5-104">Pokud jste už pomocí Windows 7 nebo Windows 8 a počítač nebo zařízení byla upgradovaná tooWindows 10, se můžete připojit pomocí nabídky nastavení hello tooAzure služby Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="896d5-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded tooWindows 10, you can join tooAzure Active Directory (Azure AD) through hello Settings menu.</span></span>

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a><span data-ttu-id="896d5-105">toojoin tooAzure AD z nabídky nastavení hello</span><span class="sxs-lookup"><span data-stu-id="896d5-105">toojoin tooAzure AD from hello Settings menu</span></span>
1. <span data-ttu-id="896d5-106">Z hello **spustit** nabídky, klikněte na tlačítko hello **nastavení** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="896d5-106">From hello **Start** menu, click hello **Settings** charm.</span></span>
2. <span data-ttu-id="896d5-107">Z **nastavení**, vyberte **systému**->**o**->**připojení k Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="896d5-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="896d5-108"><center>
   ![Připojení k Azure AD z nabídky nastavení hello](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="896d5-108"><center>
![Join Azure AD from hello Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="896d5-109">Klikněte na tlačítko **pokračovat** v okně zprávy připojení ke službě Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="896d5-109">Click **Continue** in hello Azure AD Join message window.</span></span>
   
   <span data-ttu-id="896d5-110"><center>
   ![Okno zprávy připojení k Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="896d5-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="896d5-111">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="896d5-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="896d5-112">Toto přihlášení bude obsahovat všechny hello kroky, které jsou požadované toocomplete ověřování.</span><span class="sxs-lookup"><span data-stu-id="896d5-112">This sign-in experience will include all hello steps that are required toocomplete authentication.</span></span> <span data-ttu-id="896d5-113">Pokud jste součástí federovaného klienta, váš správce vám poskytne hello federační prostředí, které hostuje vaše organizace.</span><span class="sxs-lookup"><span data-stu-id="896d5-113">If you are part of a federated tenant, your administrator will provide you with hello federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="896d5-114"><center>
   ![Zadejte přihlašovací údaje](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="896d5-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="896d5-115">Pokud vaše organizace nakonfigurovány pro připojení tooAzure AD Azure Multi-Factor Authentication, zadejte hello druhý faktor než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="896d5-115">If your organization has configured Azure Multi-Factor Authentication for joining tooAzure AD, provide hello second factor before proceeding.</span></span>
6. <span data-ttu-id="896d5-116">Klikněte na tlačítko **přijmout** na hello **povolit toobe toto zařízení spravované** obrazovky.</span><span class="sxs-lookup"><span data-stu-id="896d5-116">Click **Accept** on hello **Allow this device toobe managed** screen.</span></span>
7. <span data-ttu-id="896d5-117">Měli byste vidět hello zpráva "zařízení je teď připojený k tooyour organizace ve službě Azure AD".</span><span class="sxs-lookup"><span data-stu-id="896d5-117">You should see hello message "Your device is now joined tooyour organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="896d5-118">Další informace</span><span class="sxs-lookup"><span data-stu-id="896d5-118">Additional information</span></span>
* [<span data-ttu-id="896d5-119">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="896d5-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="896d5-120">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="896d5-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="896d5-121">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="896d5-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="896d5-122">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="896d5-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

