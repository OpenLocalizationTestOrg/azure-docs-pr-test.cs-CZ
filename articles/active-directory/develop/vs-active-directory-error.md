---
title: "chyby toodiagnose aaaHow s hello Průvodce služby Azure Active Directory připojení"
description: "Průvodce připojením Hello služby active directory zjistila typu nekompatibilní ověřování"
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
ms.openlocfilehash: f71c5b41457c0c8db05042e8d5f723e58ad11844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a><span data-ttu-id="93831-103">Diagnostikování chyb pomocí hello Průvodce služby Azure Active Directory připojení</span><span class="sxs-lookup"><span data-stu-id="93831-103">Diagnosing errors with hello Azure Active Directory Connection Wizard</span></span>
<span data-ttu-id="93831-104">Při zjišťování předchozí kód ověřování, hello Průvodce zjistil typu nekompatibilní ověřování.</span><span class="sxs-lookup"><span data-stu-id="93831-104">While detecting previous authentication code, hello wizard detected an incompatible authentication type.</span></span>   

## <a name="what-is-being-checked"></a><span data-ttu-id="93831-105">Co se kontroluje?</span><span class="sxs-lookup"><span data-stu-id="93831-105">What is being checked?</span></span>
<span data-ttu-id="93831-106">**Poznámka:** toocorrectly detekovat předchozí ověřovacího kódu v projektu, musí být vytvořená hello projektu.</span><span class="sxs-lookup"><span data-stu-id="93831-106">**Note:** toocorrectly detect previous authentication code in a project, hello project must be built.</span></span>  <span data-ttu-id="93831-107">Pokud došlo k této chybě a nemáte předchozí ověřovacího kódu v projektu, znovu sestavte a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="93831-107">If you encountered this error and you don't have a previous authentication code in your project, rebuild and try again.</span></span>

### <a name="project-types"></a><span data-ttu-id="93831-108">Typy projektů</span><span class="sxs-lookup"><span data-stu-id="93831-108">Project Types</span></span>
<span data-ttu-id="93831-109">Hello Průvodce zkontroluje hello typu projektu, které vyvíjíte, takže ji vložit hello správné ověřování logiku do projektu hello.</span><span class="sxs-lookup"><span data-stu-id="93831-109">hello wizard checks hello type of project you’re developing so it can inject hello right authentication logic into hello project.</span></span>  <span data-ttu-id="93831-110">Pokud je každý kontroler, který je odvozen od `ApiController` v projektu hello hello projektu se považuje za WebAPI projektu.</span><span class="sxs-lookup"><span data-stu-id="93831-110">If there is any controller that derives from `ApiController` in hello project, hello project is considered a WebAPI project.</span></span>  <span data-ttu-id="93831-111">Pokud jsou pouze řadiče, které jsou odvozeny od `MVC.Controller` v projektu hello hello projektu se považuje za projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="93831-111">If there are only controllers that derive from `MVC.Controller` in hello project, hello project is considered an MVC project.</span></span>  <span data-ttu-id="93831-112">Cokoliv jiného hello Průvodce nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="93831-112">Anything else is not supported by hello wizard.</span></span>

### <a name="compatible-authentication-code"></a><span data-ttu-id="93831-113">Kompatibilní ověřovací kód</span><span class="sxs-lookup"><span data-stu-id="93831-113">Compatible Authentication Code</span></span>
<span data-ttu-id="93831-114">Průvodce Hello také zkontroluje nastavení ověřování, které byly dříve nakonfigurovány pomocí Průvodce hello nebo jsou kompatibilní s průvodcem hello.</span><span class="sxs-lookup"><span data-stu-id="93831-114">hello wizard also checks for authentication settings that have been previously configured with hello wizard or are compatible with hello wizard.</span></span>  <span data-ttu-id="93831-115">Pokud jsou v něm všechna nastavení, bude považován za vícenásobně případu a otevře se Průvodce hello zobrazit nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="93831-115">If all settings are present, it is considered a re-entrant case, and hello wizard opens display hello settings.</span></span>  <span data-ttu-id="93831-116">Pokud jenom některá nastavení hello jsou v něm, bude považován za případ k chybě.</span><span class="sxs-lookup"><span data-stu-id="93831-116">If only some of hello settings are present, it is considered an error case.</span></span>

<span data-ttu-id="93831-117">V projektu MVC hello Průvodce zkontroluje pro některý z následujících nastavení, které jsou výsledkem předchozí pomocí Průvodce hello hello:</span><span class="sxs-lookup"><span data-stu-id="93831-117">In an MVC project, hello wizard checks for any of hello following settings, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

<span data-ttu-id="93831-118">Kromě toho hello Průvodce zkontroluje pro některý z následujících nastavení v projektu webového rozhraní API, které jsou výsledkem předchozí pomocí Průvodce hello hello:</span><span class="sxs-lookup"><span data-stu-id="93831-118">In addition, hello wizard checks for any of hello following settings in a Web API project, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a><span data-ttu-id="93831-119">Nekompatibilní ověřovací kód</span><span class="sxs-lookup"><span data-stu-id="93831-119">Incompatible Authentication Code</span></span>
<span data-ttu-id="93831-120">Nakonec hello průvodce se pokusí toodetect verze ověřovací kód, které byly nakonfigurovány s předchozími verzemi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93831-120">Finally, hello wizard attempts toodetect versions of authentication code that have been configured with previous versions of Visual Studio.</span></span> <span data-ttu-id="93831-121">Pokud se tato chyba, znamená to, že váš projekt obsahuje typu nekompatibilní ověřování.</span><span class="sxs-lookup"><span data-stu-id="93831-121">If you received this error, it means your project contains an incompatible authentication type.</span></span> <span data-ttu-id="93831-122">Hello průvodce rozpozná hello následující typy ověřování z předchozích verzí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="93831-122">hello wizard detects hello following types of authentication from previous versions of Visual Studio:</span></span>

* <span data-ttu-id="93831-123">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="93831-123">Windows Authentication</span></span> 
* <span data-ttu-id="93831-124">Jednotlivých uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="93831-124">Individual User Accounts</span></span> 
* <span data-ttu-id="93831-125">Účty organizace</span><span class="sxs-lookup"><span data-stu-id="93831-125">Organizational Accounts</span></span> 

<span data-ttu-id="93831-126">toodetect ověřování systému Windows v projektu aplikace MVC, hello Průvodce hledá hello `authentication` element z vaší **web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="93831-126">toodetect Windows Authentication in an MVC project, hello wizard looks for hello `authentication` element from your **web.config** file.</span></span>

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="93831-127">toodetect ověřování systému Windows v projektu webového rozhraní API, hello Průvodce hledá hello `IISExpressWindowsAuthentication` element ze svého projektu **.csproj** souboru:</span><span class="sxs-lookup"><span data-stu-id="93831-127">toodetect Windows Authentication in a Web API project, hello wizard looks for hello `IISExpressWindowsAuthentication` element from your project's **.csproj** file:</span></span>

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

<span data-ttu-id="93831-128">ověřování toodetect jednotlivé uživatelské účty, hello Průvodce hledá hello balíček element z vaší **Packages.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="93831-128">toodetect Individual User Accounts authentication, hello wizard looks for hello package element from your **Packages.config** file.</span></span>

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

<span data-ttu-id="93831-129">toodetect staré formu ověřování účtu organizace, hello Průvodce hledá hello následující element z **web.config**:</span><span class="sxs-lookup"><span data-stu-id="93831-129">toodetect an old form of Organizational Account authentication, hello wizard looks for hello following element from **web.config**:</span></span>

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="93831-130">typ ověřování hello toochange, odeberte typ nekompatibilní ověřování hello a znovu spusťte Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="93831-130">toochange hello authentication type, remove hello incompatible authentication type and run hello wizard again.</span></span>

<span data-ttu-id="93831-131">Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="93831-131">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

#<a name="next-steps"></a><span data-ttu-id="93831-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93831-132">Next steps</span></span>
- [<span data-ttu-id="93831-133">Scénáře ověřování pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="93831-133">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)