---
title: "Diagnostika chyb pomocí Průvodce Azure Active Directory připojení"
description: "Průvodce připojením k službě active directory zjistila typu nekompatibilní ověřování"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 4f29f62b2996cae98b02c1ed5fcb59eca09301ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a><span data-ttu-id="69c95-103">Diagnostikování chyb pomocí Průvodce Azure Active Directory připojení</span><span class="sxs-lookup"><span data-stu-id="69c95-103">Diagnosing errors with the Azure Active Directory Connection Wizard</span></span>
<span data-ttu-id="69c95-104">Při zjišťování předchozí kód ověřování, Průvodce zjistil typu nekompatibilní ověřování.</span><span class="sxs-lookup"><span data-stu-id="69c95-104">While detecting previous authentication code, the wizard detected an incompatible authentication type.</span></span>   

## <a name="what-is-being-checked"></a><span data-ttu-id="69c95-105">Co se kontroluje?</span><span class="sxs-lookup"><span data-stu-id="69c95-105">What is being checked?</span></span>
<span data-ttu-id="69c95-106">**Poznámka:** správně zjistit předchozí ověřovacího kódu v projektu, musí být vytvořená projektu.</span><span class="sxs-lookup"><span data-stu-id="69c95-106">**Note:** To correctly detect previous authentication code in a project, the project must be built.</span></span>  <span data-ttu-id="69c95-107">Pokud došlo k této chybě a nemáte předchozí ověřovacího kódu v projektu, znovu sestavte a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="69c95-107">If you encountered this error and you don't have a previous authentication code in your project, rebuild and try again.</span></span>

### <a name="project-types"></a><span data-ttu-id="69c95-108">Typy projektů</span><span class="sxs-lookup"><span data-stu-id="69c95-108">Project Types</span></span>
<span data-ttu-id="69c95-109">Průvodce zkontroluje typu projektu, které vyvíjíte, takže ho můžete vložit správné ověřování do projektu.</span><span class="sxs-lookup"><span data-stu-id="69c95-109">The wizard checks the type of project you’re developing so it can inject the right authentication logic into the project.</span></span>  <span data-ttu-id="69c95-110">Pokud je každý kontroler, který je odvozen od `ApiController` v projektu projekt se považuje za WebAPI projektu.</span><span class="sxs-lookup"><span data-stu-id="69c95-110">If there is any controller that derives from `ApiController` in the project, the project is considered a WebAPI project.</span></span>  <span data-ttu-id="69c95-111">Pokud jsou pouze řadiče, které jsou odvozeny od `MVC.Controller` v projektu projekt se považuje za projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="69c95-111">If there are only controllers that derive from `MVC.Controller` in the project, the project is considered an MVC project.</span></span>  <span data-ttu-id="69c95-112">Cokoliv jiného Průvodce nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="69c95-112">Anything else is not supported by the wizard.</span></span>

### <a name="compatible-authentication-code"></a><span data-ttu-id="69c95-113">Kompatibilní ověřovací kód</span><span class="sxs-lookup"><span data-stu-id="69c95-113">Compatible Authentication Code</span></span>
<span data-ttu-id="69c95-114">Průvodce také zkontroluje nastavení ověřování, které byly dříve nakonfigurovány pomocí průvodce nebo jsou kompatibilní s průvodcem.</span><span class="sxs-lookup"><span data-stu-id="69c95-114">The wizard also checks for authentication settings that have been previously configured with the wizard or are compatible with the wizard.</span></span>  <span data-ttu-id="69c95-115">Pokud jsou v něm všechna nastavení, bude považován za vícenásobně případu a otevře se průvodce zobrazit nastavení.</span><span class="sxs-lookup"><span data-stu-id="69c95-115">If all settings are present, it is considered a re-entrant case, and the wizard opens display the settings.</span></span>  <span data-ttu-id="69c95-116">Pokud jenom některá nastavení jsou v něm, bude považován za případ k chybě.</span><span class="sxs-lookup"><span data-stu-id="69c95-116">If only some of the settings are present, it is considered an error case.</span></span>

<span data-ttu-id="69c95-117">V projektu aplikace MVC Průvodce zkontroluje pro žádné z následujících nastavení, které jsou výsledkem předchozí pomocí průvodce:</span><span class="sxs-lookup"><span data-stu-id="69c95-117">In an MVC project, the wizard checks for any of the following settings, which result from previous use of the wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

<span data-ttu-id="69c95-118">Kromě toho Průvodce zkontroluje pro žádné z následujících nastavení v projektu webového rozhraní API, které jsou výsledkem předchozí pomocí průvodce:</span><span class="sxs-lookup"><span data-stu-id="69c95-118">In addition, the wizard checks for any of the following settings in a Web API project, which result from previous use of the wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a><span data-ttu-id="69c95-119">Nekompatibilní ověřovací kód</span><span class="sxs-lookup"><span data-stu-id="69c95-119">Incompatible Authentication Code</span></span>
<span data-ttu-id="69c95-120">Nakonec průvodce se pokusí zjistit verzích ověřovací kód, které byly nakonfigurovány s předchozími verzemi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69c95-120">Finally, the wizard attempts to detect versions of authentication code that have been configured with previous versions of Visual Studio.</span></span> <span data-ttu-id="69c95-121">Pokud se tato chyba, znamená to, že váš projekt obsahuje typu nekompatibilní ověřování.</span><span class="sxs-lookup"><span data-stu-id="69c95-121">If you received this error, it means your project contains an incompatible authentication type.</span></span> <span data-ttu-id="69c95-122">Průvodce rozpozná následující typy ověřování z předchozích verzí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="69c95-122">The wizard detects the following types of authentication from previous versions of Visual Studio:</span></span>

* <span data-ttu-id="69c95-123">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="69c95-123">Windows Authentication</span></span> 
* <span data-ttu-id="69c95-124">Jednotlivých uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="69c95-124">Individual User Accounts</span></span> 
* <span data-ttu-id="69c95-125">Účty organizace</span><span class="sxs-lookup"><span data-stu-id="69c95-125">Organizational Accounts</span></span> 

<span data-ttu-id="69c95-126">Ke zjištění ověřování systému Windows v projektu aplikace MVC, Průvodce hledá `authentication` element z vaší **web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="69c95-126">To detect Windows Authentication in an MVC project, the wizard looks for the `authentication` element from your **web.config** file.</span></span>

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="69c95-127">Ke zjištění ověřování systému Windows v projektu webového rozhraní API, Průvodce hledá `IISExpressWindowsAuthentication` element ze svého projektu **.csproj** souboru:</span><span class="sxs-lookup"><span data-stu-id="69c95-127">To detect Windows Authentication in a Web API project, the wizard looks for the `IISExpressWindowsAuthentication` element from your project's **.csproj** file:</span></span>

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

<span data-ttu-id="69c95-128">Ke zjištění ověřování jednotlivých uživatelských účtů, Průvodce hledá element balíček z vaší **Packages.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="69c95-128">To detect Individual User Accounts authentication, the wizard looks for the package element from your **Packages.config** file.</span></span>

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

<span data-ttu-id="69c95-129">Ke zjištění staré formu ověřování účtu organizace, Průvodce hledá následující element z **web.config**:</span><span class="sxs-lookup"><span data-stu-id="69c95-129">To detect an old form of Organizational Account authentication, the wizard looks for the following element from **web.config**:</span></span>

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="69c95-130">Chcete-li změnit typ ověřování, odeberte typ nekompatibilní ověřování a opakujte akci.</span><span class="sxs-lookup"><span data-stu-id="69c95-130">To change the authentication type, remove the incompatible authentication type and run the wizard again.</span></span>

<span data-ttu-id="69c95-131">Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="69c95-131">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

#<a name="next-steps"></a><span data-ttu-id="69c95-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69c95-132">Next steps</span></span>
- [<span data-ttu-id="69c95-133">Scénáře ověřování pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="69c95-133">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)