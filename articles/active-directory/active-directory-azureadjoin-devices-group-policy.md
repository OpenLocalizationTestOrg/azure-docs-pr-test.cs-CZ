---
title: "vyskytne aaaConnect tooAzure připojená k doméně AD pro Windows 10 | Microsoft Docs"
description: "Vysvětluje, jak správci můžou nakonfigurovat zásady skupiny tooenable zařízení toobe připojený k doméně toohello podnikové sítě."
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
ms.openlocfilehash: 9766aa702352dea2ecad3a9a0bdf8d3286ee6d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a><span data-ttu-id="ea252-103">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="ea252-103">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>
<span data-ttu-id="ea252-104">Připojení k doméně je hello tradiční způsob organizace mají připojená zařízení pro práci pro hello posledních 15 let a další.</span><span class="sxs-lookup"><span data-stu-id="ea252-104">Domain join is hello traditional way organizations have connected devices for work for hello last 15 years and more.</span></span> <span data-ttu-id="ea252-105">Aktivoval toosign uživatele v zařízení tootheir pomocí práci Windows Server Active Directory (aktivní adresář) nebo školní účty a povolených toofully IT spravovat tato zařízení.</span><span class="sxs-lookup"><span data-stu-id="ea252-105">It has enabled users toosign in tootheir devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT toofully manage these devices.</span></span> <span data-ttu-id="ea252-106">Organizace se zpravidla spoléhají na imaging metody tooprovision zařízení toousers a obecně používat toomanage System Center Configuration Manager (SCCM) nebo zásad skupiny je.</span><span class="sxs-lookup"><span data-stu-id="ea252-106">Organizations typically rely on imaging methods tooprovision devices toousers and generally use System Center Configuration Manager (SCCM) or Group Policy toomanage them.</span></span>


<span data-ttu-id="ea252-107">Připojení k doméně v systému Windows 10 vám poskytne hello po připojení zařízení tooAzure služby Active Directory (Azure AD) následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ea252-107">Domain join in Windows 10 provides you with hello following benefits after you connect devices tooAzure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="ea252-108">Jeden přihlašování (SSO) tooAzure AD prostředkům z libovolného místa</span><span class="sxs-lookup"><span data-stu-id="ea252-108">Single sign-on (SSO) tooAzure AD resources from anywhere</span></span>
* <span data-ttu-id="ea252-109">Přístup k podnikovým toohello Windows Store pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)</span><span class="sxs-lookup"><span data-stu-id="ea252-109">Access toohello enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="ea252-110">Kompatibilní se standardem Enterprise cestovní nastavení uživatele v zařízeních pomocí pracovní nebo školní účty (bez účtu Microsoft požadované)</span><span class="sxs-lookup"><span data-stu-id="ea252-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="ea252-111">Silné ověřování a pohodlný přihlášení pro pracovní nebo školní účty s Windows Hello pro firmy a Windows Hello</span><span class="sxs-lookup"><span data-stu-id="ea252-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="ea252-112">Možnost toorestrict přístup pouze toodevices, že jsou v souladu s nastaveními zásad skupiny organizační zařízení</span><span class="sxs-lookup"><span data-stu-id="ea252-112">Ability toorestrict access only toodevices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea252-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ea252-113">Prerequisites</span></span>
<span data-ttu-id="ea252-114">Připojení k doméně pokračuje toobe užitečné.</span><span class="sxs-lookup"><span data-stu-id="ea252-114">Domain join continues toobe useful.</span></span> <span data-ttu-id="ea252-115">Ale tooget hello Azure AD výhody jednotné přihlašování, cestovní nastavení s pracovní nebo školní účty a přístup k úložišti tooWindows s pracovní nebo školní účty, budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ea252-115">However, tooget hello Azure AD benefits of SSO, roaming of settings with work or school accounts, and access tooWindows Store with work or school accounts, you will need hello following:</span></span>

* <span data-ttu-id="ea252-116">Předplatné Azure AD</span><span class="sxs-lookup"><span data-stu-id="ea252-116">Azure AD subscription</span></span>
* <span data-ttu-id="ea252-117">Azure AD Connect tooextend hello místní adresář tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="ea252-117">Azure AD Connect tooextend hello on-premises directory tooAzure AD</span></span>
* <span data-ttu-id="ea252-118">Zásady, které nastavil tooAzure tooconnect připojená k doméně AD</span><span class="sxs-lookup"><span data-stu-id="ea252-118">Policy that's set tooconnect domain-joined devices tooAzure AD</span></span>
* <span data-ttu-id="ea252-119">Sestavení Windows 10 (sestavení 10551 nebo novější) pro zařízení</span><span class="sxs-lookup"><span data-stu-id="ea252-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="ea252-120">tooenable Windows Hello pro firmy a Windows Hello, budete také potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ea252-120">tooenable Windows Hello for Business and Windows Hello, you will also need hello following:</span></span>

- <span data-ttu-id="ea252-121">**Infrastruktury veřejných klíčů (PKI)** pro vystavování certifikátů uživatele.</span><span class="sxs-lookup"><span data-stu-id="ea252-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="ea252-122">**System Center Configuration Manager aktuální větve** -potřebovat tooinstall verze 1606 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="ea252-122">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span>  
<span data-ttu-id="ea252-123">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ea252-123">For more information, see:</span></span> 
    - [<span data-ttu-id="ea252-124">Dokumentace pro System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="ea252-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="ea252-125">Blog týmu System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="ea252-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="ea252-126">Windows Hello pro firmy nastavení v nástroji System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="ea252-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="ea252-127">Jako alternativní toohello infrastruktury veřejných KLÍČŮ nasazení požadavek, můžete provést následující hello:</span><span class="sxs-lookup"><span data-stu-id="ea252-127">As an alternative toohello PKI deployment requirement, you can do hello following:</span></span>

* <span data-ttu-id="ea252-128">Máte několik řadičů domény s Windows Server 2016 Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ea252-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="ea252-129">tooenable podmíněný přístup, můžete vytvořit nastavení zásad skupiny, které umožňují přístup k zařízení připojená k toodomain s žádné další nasazení.</span><span class="sxs-lookup"><span data-stu-id="ea252-129">tooenable conditional access, you can create Group Policy settings that allow access toodomain-joined devices with no additional deployments.</span></span> <span data-ttu-id="ea252-130">řízení přístupu toomanage založené na dodržování předpisů hello zařízení, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="ea252-130">toomanage access control based on compliance of hello device, you will need hello following:</span></span>

* <span data-ttu-id="ea252-131">System Center Configuration Manager aktuální větev (1606 nebo novější) pro Windows Hello pro firmy scénáře</span><span class="sxs-lookup"><span data-stu-id="ea252-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="ea252-132">Pokyny k nasazení</span><span class="sxs-lookup"><span data-stu-id="ea252-132">Deployment instructions</span></span>

<span data-ttu-id="ea252-133">toodeploy, postupujte podle kroků hello uvedených v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="ea252-133">toodeploy, follow hello steps listed in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="ea252-134">Další krok</span><span class="sxs-lookup"><span data-stu-id="ea252-134">Next step</span></span>
* [<span data-ttu-id="ea252-135">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="ea252-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="ea252-136">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="ea252-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="ea252-137">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ea252-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="ea252-138">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="ea252-138">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="ea252-139">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ea252-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

