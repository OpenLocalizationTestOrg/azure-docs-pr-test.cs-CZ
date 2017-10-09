---
title: "Začínáme s Azure AD v projektech Visual Studio MVC aaaGet | Microsoft Docs"
description: "Způsob tooget spuštění pomocí služby Azure Active Directory v projekty MVC po připojení tooor vytváření Azure AD pomocí sady Visual Studio připojené služby"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 807824dd6e4e57e443f8a7322cf2e5326384316d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Začínáme s Azure Active Directory a Visual Studio připojené služby (Projekty MVC)
> [!div class="op_single_selector"]
> * [Začínáme](vs-active-directory-dotnet-getting-started.md)
> * [Co se přihodilo](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a>Vyžadování ověřování tooaccess řadiče
Všechny řadiče ve vašem projektu byly ozdobené s hello **Autorizovat** atribut. Tento atribut vyžaduje, aby toobe hello uživatel ověřený před přístupem k tyto řadiče. tooallow hello řadiče toobe přistupovat anonymně, odeberte tento atribut z hello řadiče. Pokud chcete tooset hello oprávnění na podrobnější úrovni, použijte hello atribut tooeach metodu, která vyžaduje autorizace místo jeho použití toohello třídy kontroleru.

## <a name="adding-signin--signout-controls"></a>Přidání přihlášení / odhlášení ovládací prvky
tooadd hello přihlášení/odhlášení ovládací prvky zobrazení tooyour, můžete použít hello **_LoginPartial.cshtml** částečné zobrazení tooadd hello funkce tooone vašich zobrazení. Tady je příklad hello funkce přidané toohello standard **_Layout.cshtml** zobrazení. (Všimněte si hello posledním prvkem v hello div s navigační panel sbalí třída):

<pre>
    &lt;!DOCTYPE html&gt; 
     &lt;html&gt; 
     &lt;head&gt; 
         &lt;meta charset="utf-8" /&gt; 
        &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
        &lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") 
        @Scripts.Render("~/bundles/modernizr") 
    &lt;/head&gt; 
    &lt;body&gt; 
        &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
            &lt;div class="container"&gt; 
                &lt;div class="navbar-header"&gt; 
                    &lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                    &lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) 
                &lt;/div&gt; 
                &lt;div class="navbar-collapse collapse"&gt; 
                    &lt;ul class="nav navbar-nav"&gt; 
                        &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
                    &lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
            &lt;/div&gt; 
        &lt;/div&gt; 
        &lt;div class="container body-content"&gt; 
            @RenderBody() 
            &lt;hr /&gt; 
            &lt;footer&gt; 
                &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
            &lt;/footer&gt; 
        &lt;/div&gt; 
        @Scripts.Render("~/bundles/jquery") 
        @Scripts.Render("~/bundles/bootstrap") 
        @RenderSection("scripts", required: false) 
    &lt;/body&gt; 
    &lt;/html&gt;
</pre>

## <a name="next-steps"></a>Další kroky
- [Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 

