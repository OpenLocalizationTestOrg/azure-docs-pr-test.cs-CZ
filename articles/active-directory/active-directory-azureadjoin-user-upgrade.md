---
title: "Nastavení Windows 10 zařízení s Azure AD z nastavení | Microsoft Docs"
description: "Vysvětluje, jak mohou uživatelé připojit k Azure AD pomocí nabídky nastavení."
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
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="9617e-103">Nastavení Windows 10 zařízení s Azure AD z nastavení</span><span class="sxs-lookup"><span data-stu-id="9617e-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="9617e-104">Pokud už používáte systém Windows 7 nebo Windows 8 a počítač nebo zařízení byl upgradován na verzi Windows 10, můžete pomocí nabídky nastavení připojení k Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9617e-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded to Windows 10, you can join to Azure Active Directory (Azure AD) through the Settings menu.</span></span>

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a><span data-ttu-id="9617e-105">V nabídce nastavení připojení k Azure AD</span><span class="sxs-lookup"><span data-stu-id="9617e-105">To join to Azure AD from the Settings menu</span></span>
1. <span data-ttu-id="9617e-106">Z **spustit** nabídky, klikněte na tlačítko **nastavení** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="9617e-106">From the **Start** menu, click the **Settings** charm.</span></span>
2. <span data-ttu-id="9617e-107">Z **nastavení**, vyberte **systému**->**o**->**připojení k Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="9617e-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="9617e-108"><center>
   ![Připojení k Azure AD v nabídce nastavení](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="9617e-108"><center>
![Join Azure AD from the Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="9617e-109">Klikněte na tlačítko **pokračovat** v okně zprávy připojení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9617e-109">Click **Continue** in the Azure AD Join message window.</span></span>
   
   <span data-ttu-id="9617e-110"><center>
   ![Okno zprávy připojení k Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="9617e-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="9617e-111">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9617e-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="9617e-112">Toto přihlášení bude obsahovat všechny kroky, které jsou požadovány pro dokončení ověřování.</span><span class="sxs-lookup"><span data-stu-id="9617e-112">This sign-in experience will include all the steps that are required to complete authentication.</span></span> <span data-ttu-id="9617e-113">Pokud jste součástí federovaného klienta, váš správce vám poskytne federační prostředí, které hostuje vaše organizace.</span><span class="sxs-lookup"><span data-stu-id="9617e-113">If you are part of a federated tenant, your administrator will provide you with the federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="9617e-114"><center>
   ![Zadejte přihlašovací údaje](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="9617e-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="9617e-115">Pokud vaše organizace nakonfiguroval Azure Multi-Factor Authentication pro připojení ke službě Azure AD, zadejte druhý faktor než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="9617e-115">If your organization has configured Azure Multi-Factor Authentication for joining to Azure AD, provide the second factor before proceeding.</span></span>
6. <span data-ttu-id="9617e-116">Klikněte na tlačítko **přijmout** na **povolit toto zařízení ke správě** obrazovky.</span><span class="sxs-lookup"><span data-stu-id="9617e-116">Click **Accept** on the **Allow this device to be managed** screen.</span></span>
7. <span data-ttu-id="9617e-117">Měli byste vidět zprávu "zařízení je teď připojené k vaší organizaci ve službě Azure AD".</span><span class="sxs-lookup"><span data-stu-id="9617e-117">You should see the message "Your device is now joined to your organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="9617e-118">Další informace</span><span class="sxs-lookup"><span data-stu-id="9617e-118">Additional information</span></span>
* [<span data-ttu-id="9617e-119">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="9617e-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="9617e-120">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="9617e-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="9617e-121">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="9617e-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="9617e-122">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="9617e-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

