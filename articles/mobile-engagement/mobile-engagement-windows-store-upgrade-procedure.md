---
title: "Postupy upgradu systému Windows Universal SDK aplikace"
description: "Postupy upgradu systému Windows Universal SDK aplikací pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fe85a99a92fb39082cafe7422b356de1f20f14bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="b1fae-103">Postupy upgradu systému Windows Universal SDK aplikace</span><span class="sxs-lookup"><span data-stu-id="b1fae-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="b1fae-104">Pokud již jste spojili starší verze zapojení do své aplikace, je nutné zvážit následující body při upgradu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="b1fae-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="b1fae-105">Možná budete muset několik postupy použijte, pokud provedena několik verzí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="b1fae-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="b1fae-106">Například pokud migrujete z 0.10.1 0.11.0 budete muset nejdřív postupujte podle pokynů "od 0.9.0 k 0.10.1" pak postupu "od 0.10.1 k 0.11.0".</span><span class="sxs-lookup"><span data-stu-id="b1fae-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-330-to-340"></a><span data-ttu-id="b1fae-107">Z 3.3.0 k 3.4.0</span><span class="sxs-lookup"><span data-stu-id="b1fae-107">From 3.3.0 to 3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="b1fae-108">Protokolů testování</span><span class="sxs-lookup"><span data-stu-id="b1fae-108">Test logs</span></span>
<span data-ttu-id="b1fae-109">Protokoly konzoly vyprodukované sady SDK teď může být povolena nebo zakázána nebo filtrovat.</span><span class="sxs-lookup"><span data-stu-id="b1fae-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="b1fae-110">Chcete-li přizpůsobit tím, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z hodnota dostupná z `EngagementTestLogLevel` výčtu pro instanci:</span><span class="sxs-lookup"><span data-stu-id="b1fae-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="b1fae-111">Zdroje</span><span class="sxs-lookup"><span data-stu-id="b1fae-111">Resources</span></span>
<span data-ttu-id="b1fae-112">Bylo vylepšeno Reach překrytí.</span><span class="sxs-lookup"><span data-stu-id="b1fae-112">The Reach overlay has been improved.</span></span> <span data-ttu-id="b1fae-113">Je součástí zdroje balíčku NuGet sady SDK.</span><span class="sxs-lookup"><span data-stu-id="b1fae-113">It is part of the SDK NuGet package resources.</span></span>

<span data-ttu-id="b1fae-114">Při upgradu na novou verzi sady SDK můžete zvolit, zda chcete zachovat existující soubory ve složce překrytí vašich prostředků, nebo není:</span><span class="sxs-lookup"><span data-stu-id="b1fae-114">While upgrading to the new version of the SDK you can choose whether you want to keep your existing files from the overlay folder of your resources or not:</span></span>

* <span data-ttu-id="b1fae-115">Pokud předchozí překrytí pracuje pro vás nebo jsou integrací `WebView` elementy ručně pak můžete rozhodnout zachovat stávající soubory, ji budou i nadále fungovat.</span><span class="sxs-lookup"><span data-stu-id="b1fae-115">If the previous overlay is working for you or you are integrating the `WebView` elements manually then you can decide to keep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="b1fae-116">Pokud chcete aktualizovat na novou překrytí, právě nahradit všechny `overlay` složku z vašich prostředků s novým z balíčku SDK (aplikace UWP: Po dokončení upgradu, můžete získat novou složku překrytí % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="b1fae-116">If you want to update to the new overlay, just replace the whole `overlay` folder from your resources with the new one from the SDK package (UWP apps: after the upgrade, you can get the new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="b1fae-117">Pomocí nové překrytí přepíše všechny úpravy probíhají v předchozí verzi.</span><span class="sxs-lookup"><span data-stu-id="b1fae-117">Using the new overlay will overwrite any customizations made on the previous version.</span></span>
> 
> 

## <a name="from-320-to-330"></a><span data-ttu-id="b1fae-118">Z 3.2.0 k 3.3.0</span><span class="sxs-lookup"><span data-stu-id="b1fae-118">From 3.2.0 to 3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="b1fae-119">Zdroje</span><span class="sxs-lookup"><span data-stu-id="b1fae-119">Resources</span></span>
<span data-ttu-id="b1fae-120">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-120">This step concerns customized resources only.</span></span> <span data-ttu-id="b1fae-121">Pokud jste upravili prostředky poskytované sadě SDK (html, obrázky, překrytí) budete muset zálohování je před upgradem a znovu použít vlastní na upgradovaný prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-121">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-to-320"></a><span data-ttu-id="b1fae-122">Z 3.1.0 k 3.2.0</span><span class="sxs-lookup"><span data-stu-id="b1fae-122">From 3.1.0 to 3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="b1fae-123">Zdroje</span><span class="sxs-lookup"><span data-stu-id="b1fae-123">Resources</span></span>
<span data-ttu-id="b1fae-124">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-124">This step concerns customized resources only.</span></span> <span data-ttu-id="b1fae-125">Pokud jste upravili prostředky poskytované sadě SDK (html, obrázky, překrytí) budete muset zálohování je před upgradem a znovu použít vlastní na upgradovaný prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-125">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="b1fae-126">Integrace webové zobrazení</span><span class="sxs-lookup"><span data-stu-id="b1fae-126">Webview integration</span></span>
<span data-ttu-id="b1fae-127">Některé vylepšení tak, aby odpovídaly velikostem jiné zařízení byly zavedeny v této verzi.</span><span class="sxs-lookup"><span data-stu-id="b1fae-127">Some improvements to match different device form factors were introduced in this version.</span></span> <span data-ttu-id="b1fae-128">Zajistěte, aby odpovídaly svoji integraci webové zobrazení následující:</span><span class="sxs-lookup"><span data-stu-id="b1fae-128">Make sure that your integration of the webview match the following:</span></span>

<span data-ttu-id="b1fae-129">Ve vaší () stránky XAML:</span><span class="sxs-lookup"><span data-stu-id="b1fae-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="b1fae-130">A v souboru přidružené .cs:</span><span class="sxs-lookup"><span data-stu-id="b1fae-130">And in your associated .cs file:</span></span>

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-to-300"></a><span data-ttu-id="b1fae-131">Z 2.0.0 k 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b1fae-131">From 2.0.0 to 3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="b1fae-132">Zdroje</span><span class="sxs-lookup"><span data-stu-id="b1fae-132">Resources</span></span>
<span data-ttu-id="b1fae-133">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-133">This step concerns customized resources only.</span></span> <span data-ttu-id="b1fae-134">Pokud jste upravili prostředky poskytované sadě SDK (html, obrázky, překrytí) budete muset zálohování je před upgradem a znovu použít vlastní na upgradovaný prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-134">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-to-200"></a><span data-ttu-id="b1fae-135">Z 1.1.1 k 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b1fae-135">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="b1fae-136">Následující část popisuje postup migrace integraci sady SDK z Capptain služby, které do aplikace používá technologii Azure Mobile Engagement nabízí Capptain SAS.</span><span class="sxs-lookup"><span data-stu-id="b1fae-136">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b1fae-137">Capptain a Mobile Engagement nejsou stejné služby a postup níže uvedené jenom dozvíte, jak migrovat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1fae-137">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="b1fae-138">Migrace sady SDK v aplikaci není migrovat data ze serverů Capptain na servery Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="b1fae-138">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="b1fae-139">Pokud provádíte migraci ze starší verze, najdete na webu společnosti Capptain nejdřív přenést 1.1.1 pak použije následující postup</span><span class="sxs-lookup"><span data-stu-id="b1fae-139">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="b1fae-140">Balíček Nuget</span><span class="sxs-lookup"><span data-stu-id="b1fae-140">Nuget package</span></span>
<span data-ttu-id="b1fae-141">Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.</span><span class="sxs-lookup"><span data-stu-id="b1fae-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="b1fae-142">Použití Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="b1fae-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="b1fae-143">Sada SDK používá termín `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="b1fae-143">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="b1fae-144">Je potřeba aktualizovat projekt tak, aby odpovídaly tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="b1fae-144">You need to update your project to match this change.</span></span>

<span data-ttu-id="b1fae-145">Je potřeba odinstalovat váš aktuální Capptain balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="b1fae-145">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="b1fae-146">Zvažte, se odeberou všechny změny ve složce Capptain prostředky.</span><span class="sxs-lookup"><span data-stu-id="b1fae-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="b1fae-147">Pokud chcete, aby tyto soubory pak proveďte jejich kopii.</span><span class="sxs-lookup"><span data-stu-id="b1fae-147">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="b1fae-148">Potom nainstalujte nový balíček nuget Microsoft Azure Engagement na projektu.</span><span class="sxs-lookup"><span data-stu-id="b1fae-148">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="b1fae-149">Najdete ho přímo na [nuget webu].</span><span class="sxs-lookup"><span data-stu-id="b1fae-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="b1fae-150">nebo sem index.</span><span class="sxs-lookup"><span data-stu-id="b1fae-150">or here index.</span></span> <span data-ttu-id="b1fae-151">Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nová knihovna DLL Engagement odkazy projektu.</span><span class="sxs-lookup"><span data-stu-id="b1fae-151">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="b1fae-152">Je nutné vyčistit odkazy projektu odstraněním Capptain DLL odkazy.</span><span class="sxs-lookup"><span data-stu-id="b1fae-152">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="b1fae-153">Pokud to neuděláte, budou v konfliktu verze Capptain a dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="b1fae-153">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="b1fae-154">Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory zapojení.</span><span class="sxs-lookup"><span data-stu-id="b1fae-154">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="b1fae-155">Upozorňujeme, že soubory xaml a cs muset aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b1fae-155">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="b1fae-156">Po dokončení těchto kroků stačí nahraďte staré odkazy Capptain pomocí nové odkazy zapojení.</span><span class="sxs-lookup"><span data-stu-id="b1fae-156">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="b1fae-157">Všechny obory názvů Capptain muset aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b1fae-157">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="b1fae-158">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="b1fae-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="b1fae-159">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="b1fae-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="b1fae-160">Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="b1fae-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="b1fae-161">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="b1fae-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="b1fae-162">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="b1fae-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="b1fae-163">Pro soubory xaml Capptain obor názvů a atributy také změnit.</span><span class="sxs-lookup"><span data-stu-id="b1fae-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="b1fae-164">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="b1fae-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="b1fae-165">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="b1fae-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="b1fae-166">Změny stránky překrytí</span><span class="sxs-lookup"><span data-stu-id="b1fae-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b1fae-167">Překrytí také změní.</span><span class="sxs-lookup"><span data-stu-id="b1fae-167">Overlay also changes.</span></span> <span data-ttu-id="b1fae-168">Jeho nový obor názvů je `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="b1fae-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="b1fae-169">Je třeba použít v souboru xaml a cs.</span><span class="sxs-lookup"><span data-stu-id="b1fae-169">It has to be used in both xaml and cs files.</span></span> <span data-ttu-id="b1fae-170">Kromě toho `CapptainGrid` je s názvem `EngagementGrid`, `capptain_notification_content` a `capptain_announcement_content` jsou pojmenované `engagement_notification_content` a `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="b1fae-170">Moreover `CapptainGrid` is to be named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="b1fae-171">Pro překrytí:</span><span class="sxs-lookup"><span data-stu-id="b1fae-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="b1fae-172">Stane se:</span><span class="sxs-lookup"><span data-stu-id="b1fae-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="b1fae-173">Pro jiné prostředky jako jsou soubory HTML a obrázky Capptain Všimněte si, že se také přejmenovaná používat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="b1fae-173">For the other resources like Capptain pictures and HTML files, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="b1fae-174">Deklarace projektu</span><span class="sxs-lookup"><span data-stu-id="b1fae-174">Project declaration</span></span>
<span data-ttu-id="b1fae-175">Na Package.appxmanifest `File Type Associations` byla aktualizována z:</span><span class="sxs-lookup"><span data-stu-id="b1fae-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="b1fae-176">capptain\_dosáhnout\_obsahu engagement\_dosáhnout\_obsahu</span><span class="sxs-lookup"><span data-stu-id="b1fae-176">capptain\_reach\_content to engagement\_reach\_content</span></span>
* <span data-ttu-id="b1fae-177">capptain\_protokolu\_soubor engagement\_protokolu\_souboru</span><span class="sxs-lookup"><span data-stu-id="b1fae-177">capptain\_log\_file to engagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="b1fae-178">ID aplikace nebo klíč SDK</span><span class="sxs-lookup"><span data-stu-id="b1fae-178">Application ID / SDK Key</span></span>
<span data-ttu-id="b1fae-179">Zapojení používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="b1fae-179">Engagement uses a connection string.</span></span> <span data-ttu-id="b1fae-180">Nemáte a zadejte ID aplikace a klíč SDK s Mobile Engagement, stačí zadat připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="b1fae-180">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="b1fae-181">Můžete ho nastavit tak v souboru EngagementConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b1fae-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="b1fae-182">Konfigurace Engagement může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="b1fae-182">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="b1fae-183">Upravte tento soubor k určení:</span><span class="sxs-lookup"><span data-stu-id="b1fae-183">Edit this file to specify:</span></span>

* <span data-ttu-id="b1fae-184">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="b1fae-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="b1fae-185">Pokud chcete zadat za běhu, můžete volat metodu před Engagement inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="b1fae-185">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="b1fae-186">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="b1fae-186">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="b1fae-187">Změna názvu položky</span><span class="sxs-lookup"><span data-stu-id="b1fae-187">Items name change</span></span>
<span data-ttu-id="b1fae-188">Všechny položky s názvem *capptain* byly pojmenovány *engagement*.</span><span class="sxs-lookup"><span data-stu-id="b1fae-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="b1fae-189">Podobně jako pro *Capptain* k *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="b1fae-189">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="b1fae-190">Příklady běžně používané Capptain položek:</span><span class="sxs-lookup"><span data-stu-id="b1fae-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="b1fae-191">CapptainConfiguration nyní s názvem EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="b1fae-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="b1fae-192">Nyní s názvem EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="b1fae-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="b1fae-193">Nyní s názvem EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="b1fae-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="b1fae-194">Nyní s názvem EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="b1fae-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="b1fae-195">Nyní s názvem GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="b1fae-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="b1fae-196">Všimněte si, že přejmenovat také ovlivňuje přepsat metody.</span><span class="sxs-lookup"><span data-stu-id="b1fae-196">Note that rename also affects overridden methods.</span></span>

