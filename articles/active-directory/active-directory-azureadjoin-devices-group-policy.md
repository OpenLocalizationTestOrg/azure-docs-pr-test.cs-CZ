---
title: "Připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne | Microsoft Docs"
description: "Vysvětluje, jak může správce nakonfigurovat zásady skupiny k zařízením povolit, aby doméně k podnikové síti."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9c91579d20bb84701f6d0b97d944728c84044adf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a><span data-ttu-id="6a899-103">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="6a899-103">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>
<span data-ttu-id="6a899-104">Připojení k doméně je že tradičním způsobem, jakým organizace mají připojená zařízení pro práci pro posledních 15 let a další.</span><span class="sxs-lookup"><span data-stu-id="6a899-104">Domain join is the traditional way organizations have connected devices for work for the last 15 years and more.</span></span> <span data-ttu-id="6a899-105">Má povoleno přihlašování ke svým zařízením pomocí práci Windows Server Active Directory (aktivní adresář) nebo školní účty uživatelů a povoleny IT pro plnohodnotnou správu těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="6a899-105">It has enabled users to sign in to their devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT to fully manage these devices.</span></span> <span data-ttu-id="6a899-106">Organizace se zpravidla spoléhají na vytváření bitové kopie metody pro zřizování zařízení pro uživatele a obvykle použijte System Center Configuration Manager (SCCM) nebo zásady skupiny k jejich správě.</span><span class="sxs-lookup"><span data-stu-id="6a899-106">Organizations typically rely on imaging methods to provision devices to users and generally use System Center Configuration Manager (SCCM) or Group Policy to manage them.</span></span>


<span data-ttu-id="6a899-107">Připojení k doméně v systému Windows 10 poskytuje následující výhody po připojení zařízení k Azure Active Directory (Azure AD):</span><span class="sxs-lookup"><span data-stu-id="6a899-107">Domain join in Windows 10 provides you with the following benefits after you connect devices to Azure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="6a899-108">Jednotné přihlašování (SSO) k prostředkům Azure AD odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="6a899-108">Single sign-on (SSO) to Azure AD resources from anywhere</span></span>
* <span data-ttu-id="6a899-109">Přístup k podnikové síti Windows Store pomocí pracovní nebo školní účty (bez účtu Microsoft vyžaduje)</span><span class="sxs-lookup"><span data-stu-id="6a899-109">Access to the enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="6a899-110">Kompatibilní se standardem Enterprise cestovní nastavení uživatele v zařízeních pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)</span><span class="sxs-lookup"><span data-stu-id="6a899-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="6a899-111">Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Windows Hello pro firmy a Windows Hello</span><span class="sxs-lookup"><span data-stu-id="6a899-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="6a899-112">Umožňuje omezit přístup jenom na zařízení, které jsou v souladu s nastaveními zásad skupiny organizační zařízení</span><span class="sxs-lookup"><span data-stu-id="6a899-112">Ability to restrict access only to devices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a899-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a899-113">Prerequisites</span></span>
<span data-ttu-id="6a899-114">Připojení k doméně je nadále užitečné.</span><span class="sxs-lookup"><span data-stu-id="6a899-114">Domain join continues to be useful.</span></span> <span data-ttu-id="6a899-115">Ale na získat výhody Azure AD jednotné přihlašování, cestovní nastavení s pracovní nebo školní účty a přístup k Windows Store s pracovní nebo školní účty, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="6a899-115">However, to get the Azure AD benefits of SSO, roaming of settings with work or school accounts, and access to Windows Store with work or school accounts, you will need the following:</span></span>

* <span data-ttu-id="6a899-116">Předplatné Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a899-116">Azure AD subscription</span></span>
* <span data-ttu-id="6a899-117">Azure AD Connect místní adresář rozšířit do Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a899-117">Azure AD Connect to extend the on-premises directory to Azure AD</span></span>
* <span data-ttu-id="6a899-118">Zásady, které má nastavený na připojení zařízení připojených k doméně ke službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a899-118">Policy that's set to connect domain-joined devices to Azure AD</span></span>
* <span data-ttu-id="6a899-119">Sestavení Windows 10 (sestavení 10551 nebo novější) pro zařízení</span><span class="sxs-lookup"><span data-stu-id="6a899-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="6a899-120">Pokud chcete povolit Windows Hello pro firmy a Windows Hello, budete také potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="6a899-120">To enable Windows Hello for Business and Windows Hello, you will also need the following:</span></span>

- <span data-ttu-id="6a899-121">**Infrastruktury veřejných klíčů (PKI)** pro vystavování certifikátů uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a899-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="6a899-122">**System Center Configuration Manager aktuální větev** – je potřeba nainstalovat verzi 1606 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="6a899-122">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span>  
<span data-ttu-id="6a899-123">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="6a899-123">For more information, see:</span></span> 
    - [<span data-ttu-id="6a899-124">Dokumentace pro System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="6a899-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="6a899-125">Blog týmu System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="6a899-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="6a899-126">Windows Hello pro firmy nastavení v nástroji System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="6a899-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="6a899-127">Jako alternativu k požadavcích nasazení infrastruktury veřejných KLÍČŮ můžete provést následující:</span><span class="sxs-lookup"><span data-stu-id="6a899-127">As an alternative to the PKI deployment requirement, you can do the following:</span></span>

* <span data-ttu-id="6a899-128">Máte několik řadičů domény s Windows Server 2016 Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="6a899-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="6a899-129">Chcete-li povolit podmíněný přístup, můžete vytvořit nastavení zásad skupiny, které umožňují přístup k zařízení připojených k doméně pomocí žádné další nasazení.</span><span class="sxs-lookup"><span data-stu-id="6a899-129">To enable conditional access, you can create Group Policy settings that allow access to domain-joined devices with no additional deployments.</span></span> <span data-ttu-id="6a899-130">Ke správě řízení přístupu podle stavu kompatibility zařízení, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="6a899-130">To manage access control based on compliance of the device, you will need the following:</span></span>

* <span data-ttu-id="6a899-131">System Center Configuration Manager aktuální větev (1606 nebo novější) pro Windows Hello pro firmy scénáře</span><span class="sxs-lookup"><span data-stu-id="6a899-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="6a899-132">Pokyny k nasazení</span><span class="sxs-lookup"><span data-stu-id="6a899-132">Deployment instructions</span></span>

<span data-ttu-id="6a899-133">Pokud chcete nasadit, postupujte podle kroků uvedených v [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="6a899-133">To deploy, follow the steps listed in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="6a899-134">Další krok</span><span class="sxs-lookup"><span data-stu-id="6a899-134">Next step</span></span>
* [<span data-ttu-id="6a899-135">Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="6a899-135">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="6a899-136">Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="6a899-136">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="6a899-137">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="6a899-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="6a899-138">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="6a899-138">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="6a899-139">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="6a899-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

