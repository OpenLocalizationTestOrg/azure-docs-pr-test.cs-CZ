---
title: "aaaSet si nové zařízení s Azure AD během instalace | Microsoft Docs"
description: "Téma, které vysvětluje, jak uživatelé mohou vytvořit připojení ke službě Azure AD během jejich prostředí prvního spuštění."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a><span data-ttu-id="808db-103">Nastavit nové zařízení s Azure AD během instalace</span><span class="sxs-lookup"><span data-stu-id="808db-103">Set up a new device with Azure AD during Setup</span></span>
<span data-ttu-id="808db-104">V systému Windows 10 uživatelé mohou připojit své zařízení tooAzure služby Active Directory (Azure AD) v hello při prvním spuštění (FRX).</span><span class="sxs-lookup"><span data-stu-id="808db-104">In Windows 10, users can join their devices tooAzure Active Directory (Azure AD) in hello first-run experience (FRX).</span></span> <span data-ttu-id="808db-105">To umožňuje organizacím toodistribute pečlivě zabaleny zařízení tootheir zaměstnanci a studenti, kteří, nebo nechat je zvolte svá vlastní zařízení (CYOD).</span><span class="sxs-lookup"><span data-stu-id="808db-105">This allows organizations toodistribute shrink-wrapped devices tootheir employees or students, or let them choose their own devices (CYOD).</span></span>
<span data-ttu-id="808db-106">Pokud edice Windows 10 Professional nebo Windows 10 Enterprise je nainstalovaná na zařízení, hello prostředí procesu instalace toohello výchozí nastavení pro zařízení vlastněná společností.</span><span class="sxs-lookup"><span data-stu-id="808db-106">If either Windows 10 Professional or Windows 10 Enterprise editions is installed on a device, hello experience defaults toohello setup process for company-owned devices.</span></span>

## <a name="toojoin-a-device-tooazure-ad"></a><span data-ttu-id="808db-107">toojoin zařízení tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="808db-107">toojoin a device tooAzure AD</span></span>
1. <span data-ttu-id="808db-108">Při zapnutí nové zařízení a spustit proces instalace hello, měli byste vidět hello **získávání připravené** zprávy.</span><span class="sxs-lookup"><span data-stu-id="808db-108">When you turn on your new device and start hello setup process, you should see hello  **Getting Ready** message.</span></span> <span data-ttu-id="808db-109">Postupujte podle výzvy tooset hello daného zařízení.</span><span class="sxs-lookup"><span data-stu-id="808db-109">Follow hello prompts tooset up your device.</span></span>
2. <span data-ttu-id="808db-110">Spuštění zadáním oblast a jazyk.</span><span class="sxs-lookup"><span data-stu-id="808db-110">Start by customizing your region and language.</span></span> <span data-ttu-id="808db-111">Přijměte licenční podmínky softwaru společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="808db-111">Then accept hello Microsoft Software License Terms.</span></span>
   <span data-ttu-id="808db-112">![Přizpůsobit pro vaši oblast](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span><span class="sxs-lookup"><span data-stu-id="808db-112">![Customize for your region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span></span>
3. <span data-ttu-id="808db-113">Vyberte síť hello toouse chcete pro připojení toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="808db-113">Select hello network you want toouse for connecting toohello Internet.</span></span>
4. <span data-ttu-id="808db-114">Vyberte, zda používáte osobní zařízení nebo zařízení ve vlastnictví společnosti.</span><span class="sxs-lookup"><span data-stu-id="808db-114">Select whether you're using a personal device or a company-owned device.</span></span> <span data-ttu-id="808db-115">Pokud je ve vlastnictví společnosti, klikněte na tlačítko **toto zařízení patří organizaci toomy**.</span><span class="sxs-lookup"><span data-stu-id="808db-115">If it's company-owned, click **This device belongs toomy organization**.</span></span> <span data-ttu-id="808db-116">Spustí se prostředí Azure AD Join hello.</span><span class="sxs-lookup"><span data-stu-id="808db-116">This starts hello Azure AD Join experience.</span></span> <span data-ttu-id="808db-117">Toto je na obrazovce, že se zobrazí, pokud používáte Windows 10 Professional.</span><span class="sxs-lookup"><span data-stu-id="808db-117">Following is a screen that you'll see if you're using Windows 10 Professional.</span></span>
   <span data-ttu-id="808db-118"><center>
   ![Kdo je vlastníkem této obrazovce počítače](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span><span class="sxs-lookup"><span data-stu-id="808db-118"><center>
![Who owns this PC screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span></span>
5. <span data-ttu-id="808db-119">Zadejte přihlašovací údaje hello, které byly poskytnuty tooyou vaší organizací.</span><span class="sxs-lookup"><span data-stu-id="808db-119">Enter hello credentials that were provided tooyou by your organization.</span></span>
   <span data-ttu-id="808db-120"><center>
   ![Přihlašovací obrazovky](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span><span class="sxs-lookup"><span data-stu-id="808db-120"><center>
![Sign-in screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span></span>
6. <span data-ttu-id="808db-121">Po zadání uživatelského jména, odpovídající klienta se nachází ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="808db-121">After you have entered your user name, a matching tenant is located in Azure AD.</span></span> <span data-ttu-id="808db-122">Pokud jste v federovanou doménu, bude přesměrované tooyour na místním serveru zabezpečení tokenu služby (STS) – například Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="808db-122">If you are in a federated domain, you will be redirected tooyour on-premises Secure Token Service (STS) server--for example, Active Directory Federation Services (AD FS).</span></span>
7. <span data-ttu-id="808db-123">Pokud jste uživatele v doméně nefederovaných, zadejte přihlašovací údaje přímo na hello stránky Azure AD hostované.</span><span class="sxs-lookup"><span data-stu-id="808db-123">If you are a user in a non-federated domain, enter your credentials directly on hello Azure AD-hosted page.</span></span> <span data-ttu-id="808db-124">Pokud bylo nakonfigurované firemní branding, uvidíte také logo vaší organizace a podporují text.</span><span class="sxs-lookup"><span data-stu-id="808db-124">If company branding was configured, you will also see your organization’s logo and support text.</span></span>
8. <span data-ttu-id="808db-125">Se zobrazí výzva k výzvy ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="808db-125">You're prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="808db-126">Tento problém je možné konfigurovat správcem IT.</span><span class="sxs-lookup"><span data-stu-id="808db-126">This challenge is configurable by an IT administrator.</span></span>
9. <span data-ttu-id="808db-127">Azure AD ověří, jestli tento uživatel nebo zařízení vyžaduje registraci správy mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="808db-127">Azure AD checks whether this user/device requires enrollment in mobile device management.</span></span>
10. <span data-ttu-id="808db-128">Windows hello zařízení registruje v adresáři organizace hello ve službě Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="808db-128">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
11. <span data-ttu-id="808db-129">Pokud jste spravované uživatele, trvá Windows desktop toohello procesem hello automatické přihlášení.</span><span class="sxs-lookup"><span data-stu-id="808db-129">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in process.</span></span>
12. <span data-ttu-id="808db-130">Pokud jste federované uživatele, jsou směrované toohello přihlášení Windows obrazovky tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="808db-130">If you are a federated user, you are directed toohello Windows sign-in screen tooenter your credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="808db-131">Připojení k místní doméně systému Windows Server Active Directory v systému Windows hello out-of-box prostředí nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="808db-131">Joining an on-premises Windows Server Active Directory domain in hello Windows out-of-box experience is not supported.</span></span> <span data-ttu-id="808db-132">Proto pokud máte v plánu toojoin tooa domény počítače, měli byste vybrat hello odkaz **Windows nastavte s místním účtem** místo.</span><span class="sxs-lookup"><span data-stu-id="808db-132">Therefore, if you plan toojoin a computer tooa domain, you should select hello link **Set up Windows with a local account** instead.</span></span> <span data-ttu-id="808db-133">Ve vašem počítači může potom připojí hello domény z hello nastavení, které provedete krok před.</span><span class="sxs-lookup"><span data-stu-id="808db-133">You can then join hello domain from hello settings on your computer as you’ve done before.</span></span>
> 
> 

## <a name="additional-information"></a><span data-ttu-id="808db-134">Další informace</span><span class="sxs-lookup"><span data-stu-id="808db-134">Additional information</span></span>
* [<span data-ttu-id="808db-135">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="808db-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="808db-136">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="808db-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="808db-137">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="808db-137">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="808db-138">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="808db-138">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="808db-139">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="808db-139">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="808db-140">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="808db-140">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

