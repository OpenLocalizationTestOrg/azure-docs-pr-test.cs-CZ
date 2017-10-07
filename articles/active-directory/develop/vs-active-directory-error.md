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
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a>Diagnostikování chyb pomocí hello Průvodce služby Azure Active Directory připojení
Při zjišťování předchozí kód ověřování, hello Průvodce zjistil typu nekompatibilní ověřování.   

## <a name="what-is-being-checked"></a>Co se kontroluje?
**Poznámka:** toocorrectly detekovat předchozí ověřovacího kódu v projektu, musí být vytvořená hello projektu.  Pokud došlo k této chybě a nemáte předchozí ověřovacího kódu v projektu, znovu sestavte a zkuste to znovu.

### <a name="project-types"></a>Typy projektů
Hello Průvodce zkontroluje hello typu projektu, které vyvíjíte, takže ji vložit hello správné ověřování logiku do projektu hello.  Pokud je každý kontroler, který je odvozen od `ApiController` v projektu hello hello projektu se považuje za WebAPI projektu.  Pokud jsou pouze řadiče, které jsou odvozeny od `MVC.Controller` v projektu hello hello projektu se považuje za projektu MVC.  Cokoliv jiného hello Průvodce nepodporuje.

### <a name="compatible-authentication-code"></a>Kompatibilní ověřovací kód
Průvodce Hello také zkontroluje nastavení ověřování, které byly dříve nakonfigurovány pomocí Průvodce hello nebo jsou kompatibilní s průvodcem hello.  Pokud jsou v něm všechna nastavení, bude považován za vícenásobně případu a otevře se Průvodce hello zobrazit nastavení hello.  Pokud jenom některá nastavení hello jsou v něm, bude považován za případ k chybě.

V projektu MVC hello Průvodce zkontroluje pro některý z následujících nastavení, které jsou výsledkem předchozí pomocí Průvodce hello hello:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Kromě toho hello Průvodce zkontroluje pro některý z následujících nastavení v projektu webového rozhraní API, které jsou výsledkem předchozí pomocí Průvodce hello hello:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Nekompatibilní ověřovací kód
Nakonec hello průvodce se pokusí toodetect verze ověřovací kód, které byly nakonfigurovány s předchozími verzemi sady Visual Studio. Pokud se tato chyba, znamená to, že váš projekt obsahuje typu nekompatibilní ověřování. Hello průvodce rozpozná hello následující typy ověřování z předchozích verzí sady Visual Studio:

* Ověřování systému Windows 
* Jednotlivých uživatelských účtů 
* Účty organizace 

toodetect ověřování systému Windows v projektu aplikace MVC, hello Průvodce hledá hello `authentication` element z vaší **web.config** souboru.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

toodetect ověřování systému Windows v projektu webového rozhraní API, hello Průvodce hledá hello `IISExpressWindowsAuthentication` element ze svého projektu **.csproj** souboru:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

ověřování toodetect jednotlivé uživatelské účty, hello Průvodce hledá hello balíček element z vaší **Packages.config** souboru.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

toodetect staré formu ověřování účtu organizace, hello Průvodce hledá hello následující element z **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

typ ověřování hello toochange, odeberte typ nekompatibilní ověřování hello a znovu spusťte Průvodce hello.

Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Další kroky
- [Scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)