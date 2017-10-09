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
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a><span data-ttu-id="5c220-103">Začínáme s Azure Active Directory a Visual Studio připojené služby (Projekty MVC)</span><span class="sxs-lookup"><span data-stu-id="5c220-103">Getting Started with Azure Active Directory and Visual Studio connected services (MVC Projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c220-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="5c220-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="5c220-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="5c220-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a><span data-ttu-id="5c220-106">Vyžadování ověřování tooaccess řadiče</span><span class="sxs-lookup"><span data-stu-id="5c220-106">Requiring authentication tooaccess controllers</span></span>
<span data-ttu-id="5c220-107">Všechny řadiče ve vašem projektu byly ozdobené s hello **Autorizovat** atribut.</span><span class="sxs-lookup"><span data-stu-id="5c220-107">All controllers in your project were adorned with hello **Authorize** attribute.</span></span> <span data-ttu-id="5c220-108">Tento atribut vyžaduje, aby toobe hello uživatel ověřený před přístupem k tyto řadiče.</span><span class="sxs-lookup"><span data-stu-id="5c220-108">This attribute requires hello user toobe authenticated before accessing these controllers.</span></span> <span data-ttu-id="5c220-109">tooallow hello řadiče toobe přistupovat anonymně, odeberte tento atribut z hello řadiče.</span><span class="sxs-lookup"><span data-stu-id="5c220-109">tooallow hello controller toobe accessed anonymously, remove this attribute from hello controller.</span></span> <span data-ttu-id="5c220-110">Pokud chcete tooset hello oprávnění na podrobnější úrovni, použijte hello atribut tooeach metodu, která vyžaduje autorizace místo jeho použití toohello třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="5c220-110">If you want tooset hello permissions at a more granular level, apply hello attribute tooeach method that requires authorization instead of applying it toohello controller class.</span></span>

## <a name="adding-signin--signout-controls"></a><span data-ttu-id="5c220-111">Přidání přihlášení / odhlášení ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="5c220-111">Adding SignIn / SignOut Controls</span></span>
<span data-ttu-id="5c220-112">tooadd hello přihlášení/odhlášení ovládací prvky zobrazení tooyour, můžete použít hello **_LoginPartial.cshtml** částečné zobrazení tooadd hello funkce tooone vašich zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5c220-112">tooadd hello SignIn/SignOut controls tooyour view, you can use hello **_LoginPartial.cshtml** partial view tooadd hello functionality tooone of your views.</span></span> <span data-ttu-id="5c220-113">Tady je příklad hello funkce přidané toohello standard **_Layout.cshtml** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5c220-113">Here is an example of hello functionality added toohello standard **_Layout.cshtml** view.</span></span> <span data-ttu-id="5c220-114">(Všimněte si hello posledním prvkem v hello div s navigační panel sbalí třída):</span><span class="sxs-lookup"><span data-stu-id="5c220-114">(Note hello last element in hello div with class navbar-collapse):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5c220-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c220-115">Next steps</span></span>
- [<span data-ttu-id="5c220-116">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c220-116">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/) 

