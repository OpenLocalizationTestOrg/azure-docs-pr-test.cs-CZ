---
title: "Problém instalace přístup k aplikaci panelu rozšíření prohlížeče | Microsoft Docs"
description: "Jak opravit běžné chyby při instalaci rozšíření přístup k panelu prohlížeče"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 8b7327508633e33917d1fa9c1f35ed1bde5a26e1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a><span data-ttu-id="50e96-103">Rozšíření prohlížeče panelu problém instalace přístup k aplikaci</span><span class="sxs-lookup"><span data-stu-id="50e96-103">Problem installing the application access panel browser extension</span></span>

<span data-ttu-id="50e96-104">Přístupový Panel je webový portál, který uživatel, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) umožňuje zobrazovat a spouštět cloudové aplikace, které je správce Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="50e96-104">The Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) to view and launch cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="50e96-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="50e96-105">A user who has Azure AD editions can also use self-service group and app management capabilities through the Access Panel.</span></span> <span data-ttu-id="50e96-106">Přístupový Panel je nezávislý na portálu Azure a nevyžaduje, aby uživatelé měli předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="50e96-106">The Access Panel is separate from the Azure portal and does not require users to have an Azure subscription.</span></span>

<span data-ttu-id="50e96-107">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="50e96-107">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="50e96-108">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="50e96-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="50e96-109">Splňuje požadavky na prohlížeč pro přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="50e96-109">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="50e96-110">Přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="50e96-110">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="50e96-111">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="50e96-111">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="50e96-112">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="50e96-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="50e96-113">Pro jednotné přihlašování založené na heslech může být prohlížeče koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="50e96-113">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="50e96-114">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="50e96-114">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="50e96-115">Hraniční Windows 10 Anniversary Edition nebo novější.</span><span class="sxs-lookup"><span data-stu-id="50e96-115">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="50e96-116">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="50e96-116">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="50e96-117">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="50e96-117">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="50e96-118">Postup instalace rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="50e96-118">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="50e96-119">Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="50e96-119">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="50e96-120">Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50e96-120">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="50e96-121">Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="50e96-121">Click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="50e96-122">V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="50e96-122">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="50e96-123">Založené na prohlížeči budete přesměrováni na odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="50e96-123">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="50e96-124">**Přidat** rozšíření do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="50e96-124">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="50e96-125">Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="50e96-125">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="50e96-126">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="50e96-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="50e96-127">Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="50e96-127">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="50e96-128">Rozšíření pro Chrome a hraniční může také stáhnout z přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="50e96-128">You may also download the extension for Chrome and Edge from the direct links below:</span></span>

-   [<span data-ttu-id="50e96-129">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="50e96-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="50e96-130">Rozšíření panelu přístup Edge</span><span class="sxs-lookup"><span data-stu-id="50e96-130">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="50e96-131">Nastavení zásad skupiny pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="50e96-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="50e96-132">Můžete nastavit zásady skupiny, které vám umožní vzdáleně instalovat rozšíření Panel přístupu pro Internet Explorer na počítače uživatelů.</span><span class="sxs-lookup"><span data-stu-id="50e96-132">You can setup a group policy that allow you to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="50e96-133">Požadavky patří:</span><span class="sxs-lookup"><span data-stu-id="50e96-133">The prerequisites include:</span></span>

-   <span data-ttu-id="50e96-134">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a vaši uživatelé počítačů se připojili k vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="50e96-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>

-   <span data-ttu-id="50e96-135">Musíte mít oprávnění "Upravit nastavení" k úpravě objektu zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="50e96-135">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="50e96-136">Ve výchozím nastavení, toto oprávnění mají členy z těchto skupin zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="50e96-136">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="50e96-137">[Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="50e96-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="50e96-138">Postupujte podle kurzu [nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md) pro podrobné informace o tom, jak nakonfigurovat zásady skupiny a nasadit je uživatelům.</span><span class="sxs-lookup"><span data-stu-id="50e96-138">Follow the tutorial [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md) for step by step instructions on how to configure the group policy and deploy it to users.</span></span>

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a><span data-ttu-id="50e96-139">Řešení potíží s přístupového panelu v aplikaci Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="50e96-139">Troubleshoot the Access Panel in Internet Explorer</span></span>

<span data-ttu-id="50e96-140">Postupujte podle [řešení potíží s příponou Panel přístupu pro Internet Explorer](active-directory-saas-ie-troubleshooting.md) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci rozšíření pro aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="50e96-140">Follow the [Troubleshoot the Access Panel Extension for Internet Explorer](active-directory-saas-ie-troubleshooting.md) guide for access a diagnostics tool and step by step instructions on configuring the extension for IE.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a><span data-ttu-id="50e96-141">Pokud tyto kroky řešení potíží problém nevyřeší</span><span class="sxs-lookup"><span data-stu-id="50e96-141">If these troubleshooting steps do not resolve the issue</span></span>

<span data-ttu-id="50e96-142">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="50e96-142">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="50e96-143">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="50e96-143">Correlation error ID</span></span>

-   <span data-ttu-id="50e96-144">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="50e96-144">UPN (user email address)</span></span>

-   <span data-ttu-id="50e96-145">TenantID</span><span class="sxs-lookup"><span data-stu-id="50e96-145">TenantID</span></span>

-   <span data-ttu-id="50e96-146">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="50e96-146">Browser type</span></span>

-   <span data-ttu-id="50e96-147">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="50e96-147">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="50e96-148">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="50e96-148">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="50e96-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50e96-149">Next steps</span></span>
[<span data-ttu-id="50e96-150">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="50e96-150">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
