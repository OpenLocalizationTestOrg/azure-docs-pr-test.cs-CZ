---
title: "Zabezpečení aplikacím a prostředkům v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak zamknout aplikacím a prostředkům v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7fbade87-a453-426d-bfa5-c72227ea83cd
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 1c052906788f0f4fe4ca9fd6d3af63336245174a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a><span data-ttu-id="a8b80-103">Zabezpečení aplikacím a prostředkům v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="a8b80-103">Secure apps and resources in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a8b80-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="a8b80-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a8b80-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="a8b80-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a8b80-106">Azure RemoteApp poskytuje uživatelům přístup k centrálně spravované aplikace pro Windows, který vám umožňuje řídit, co uživatelé uvidí a nelze provést.</span><span class="sxs-lookup"><span data-stu-id="a8b80-106">Azure RemoteApp provides users access to centrally-managed Windows apps, which lets you control what your users can and can't do.</span></span>  <span data-ttu-id="a8b80-107">To je zvlášť užitečné, když se uživatel připojuje z nespravovaných zařízení (jako jsou jejich osobní Macbook) a chcete řídit přístup uživatele nebo prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8b80-107">This is particularly useful when the user is connecting from an unmanaged device (like their personal Macbook) and you want to control the user access or experience.</span></span>

<span data-ttu-id="a8b80-108">Například pokud používáte k ověřování uživatelů služby Active Directory a chcete zabránit uživatelům v kopírování dat z aplikace, můžete nakonfigurovat zásady skupiny Vzdálená plocha pro blokování uživatele z kopírování dat.</span><span class="sxs-lookup"><span data-stu-id="a8b80-108">For example, if you are using Active Directory for user authentication and you want to prevent your users from copying data out of an app, you can configure a Remote Desktop Group Policy to block users from copying data.</span></span>

<span data-ttu-id="a8b80-109">Dalším příkladem je, pokud chcete blokovat přístup k Internetu pro konkrétní aplikace v kolekci.</span><span class="sxs-lookup"><span data-stu-id="a8b80-109">Another example is if you want to block internet access for a particular app in your collection.</span></span> <span data-ttu-id="a8b80-110">Můžete vytvořit pravidlo brány Windows Firewall, které blokuje přístup při vytvoření bitové kopie pro vaše kolekce.</span><span class="sxs-lookup"><span data-stu-id="a8b80-110">You can create a Windows Firewall rule that blocks the access when you create the image for your collection.</span></span>

## <a name="implementation-options"></a><span data-ttu-id="a8b80-111">Možnosti implementace</span><span class="sxs-lookup"><span data-stu-id="a8b80-111">Implementation options</span></span>
  <span data-ttu-id="a8b80-112">Zde jsou možnosti klíče implementace, které můžete použít samostatně nebo v kombinaci podle potřeby:</span><span class="sxs-lookup"><span data-stu-id="a8b80-112">Here are the key implementation options, which can be used individually or in tandem as needed:</span></span>

1. <span data-ttu-id="a8b80-113">Pokud vaše kolekce vzdálené aplikace RemoteApp je připojený k doméně můžete vynutit žádné [zásad skupiny](https://technet.microsoft.com/library/cc725828.aspx) (s výjimkou zásady časový limit nečinnosti a odpojení popsané [sem](../azure-subscription-service-limits.md)).</span><span class="sxs-lookup"><span data-stu-id="a8b80-113">If your RemoteApp collection is domain joined you can enforce any [Group Policy](https://technet.microsoft.com/library/cc725828.aspx) (with the exception of the Idle and Disconnect timeout policies described [here](../azure-subscription-service-limits.md)).</span></span>
2. <span data-ttu-id="a8b80-114">Jako alternativu k zásad skupiny (Pokud kolekce není připojený k doméně, nebo nemáte správná oprávnění ve službě AD), můžete nakonfigurovat [místní zásady](https://technet.microsoft.com/library/cc775702.aspx) do image šablony.</span><span class="sxs-lookup"><span data-stu-id="a8b80-114">As an alternative to Group Policy (if your collection is not domain joined or you don't have the right privileges in AD), you can configure [Local Polices](https://technet.microsoft.com/library/cc775702.aspx) into your template image.</span></span>  <span data-ttu-id="a8b80-115">Všimněte si, že tuto skupinu zásady trumfy místní zásady, když dojde ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="a8b80-115">Note that group polices trump local policies when there is a conflict.</span></span>
3. <span data-ttu-id="a8b80-116">Některá nastavení operačního systému nebo aplikace se nedají konfigurovat prostřednictvím zásad, ale mohou být prostřednictvím pomocí klíče registru [nástroj RegEdit](remoteapp-hybridtrouble.md) při konfiguraci image šablony.</span><span class="sxs-lookup"><span data-stu-id="a8b80-116">Some OS/app settings are not configurable via policy, but can be via registry key using the [RegEdit tool](remoteapp-hybridtrouble.md) while configuring your template image.</span></span>
4. <span data-ttu-id="a8b80-117">Můžete použít [brány Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) do ovládacího prvku do a z počítače, na kterém je spuštěna aplikace přístup k síti.</span><span class="sxs-lookup"><span data-stu-id="a8b80-117">You can use [Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) to control network access to and from the machine where the app is running.</span></span> <span data-ttu-id="a8b80-118">Ujistěte se, že nedošlo k blokování adres URL a portů, které jsou definované v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="a8b80-118">Just make sure you don't block the URLs and ports defined here.</span></span>
5. <span data-ttu-id="a8b80-119">Můžete použít [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) řídit, kteří uživatelé aplikací a souborů můžete spustit.</span><span class="sxs-lookup"><span data-stu-id="a8b80-119">You can use [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) to control which applications and files users can run.</span></span> <span data-ttu-id="a8b80-120">Uživatele můžete například zjistit, jak spouštět aplikace, aby nemohl publikovat, ale které jsou k dispozici v bitové kopii, kterou jste použili k vytvoření kolekce – nástroje AppLocker můžete blokovat to.</span><span class="sxs-lookup"><span data-stu-id="a8b80-120">For example, savvy users can figure out how to run applications that you did not publish but that are available in the image you used to create the collection - AppLocker can block this.</span></span>

## <a name="detailed-information"></a><span data-ttu-id="a8b80-121">Podrobné informace</span><span class="sxs-lookup"><span data-stu-id="a8b80-121">Detailed information</span></span>
* <span data-ttu-id="a8b80-122">Tyto zásady vzdálené plochy by mohly být velmi užitečné:</span><span class="sxs-lookup"><span data-stu-id="a8b80-122">The following RDS policies are likely to be most useful:</span></span>
  * [<span data-ttu-id="a8b80-123">Zařízení a prostředků</span><span class="sxs-lookup"><span data-stu-id="a8b80-123">Device and Resource Redirection</span></span>](https://technet.microsoft.com/library/ee791794.aspx)
  * [<span data-ttu-id="a8b80-124">Přesměrování tiskárny</span><span class="sxs-lookup"><span data-stu-id="a8b80-124">Printer Redirection</span></span>](https://technet.microsoft.com/library/ee791784.aspx)
  * <span data-ttu-id="a8b80-125">[Profily](https://technet.microsoft.com/library/ee791865.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8b80-125">[Profiles](https://technet.microsoft.com/library/ee791865.aspx).</span></span>
* <span data-ttu-id="a8b80-126">Všimněte si, že konfigurace přesměrování přes modul vzdálené aplikace RemoteApp PowerShell (jak je vidět [sem](remoteapp-redirection.md)) závisí na klientském počítači. Chcete-li tyto zásady vynutit, aby primární cílem je zabezpečení budete chtít vynutit zásady prostřednictvím image šablony místní zásady nebo prostřednictvím zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="a8b80-126">Note that configuring redirections via the RemoteApp PowerShell module (as seen [here](remoteapp-redirection.md)) relies on the client machine to enforce the policy, so if security is the primary objective you'll want to enforce the policy via the template image local policy or via group policy.</span></span>
* <span data-ttu-id="a8b80-127">[Windows Server 2012 R2 zásady](https://technet.microsoft.com/library/hh831791.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8b80-127">[Windows Server 2012 R2 policies](https://technet.microsoft.com/library/hh831791.aspx).</span></span>
* <span data-ttu-id="a8b80-128">[Office 2013 zásady](https://technet.microsoft.com/library/cc178969.aspx) (včetně [postup přizpůsobení panelu nástrojů Office](https://technet.microsoft.com/library/cc179143.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a8b80-128">[Office 2013 polices](https://technet.microsoft.com/library/cc178969.aspx) (including [how to customize the Office toolbar](https://technet.microsoft.com/library/cc179143.aspx)).</span></span>

