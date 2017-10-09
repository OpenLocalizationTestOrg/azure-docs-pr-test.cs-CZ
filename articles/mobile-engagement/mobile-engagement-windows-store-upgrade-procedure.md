---
title: "aaaWindows univerzální aplikace SDK Upgrade procedury"
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
ms.openlocfilehash: 95aba5d55cd65d4190aad35737f872414b5ed443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="ea0e2-103">Postupy upgradu systému Windows Universal SDK aplikace</span><span class="sxs-lookup"><span data-stu-id="ea0e2-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="ea0e2-104">Pokud již mít integrovanou starší verze zapojení do své aplikace, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="ea0e2-105">Toofollow může mít několik postupů, pokud provedena několik verzí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="ea0e2-106">Například pokud migrujete z 0.10.1 too0.11.0 máte toofirst postupujte podle hello "z 0.9.0 too0.10.1" postup pak hello "z 0.10.1 too0.11.0" postup.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-330-too340"></a><span data-ttu-id="ea0e2-107">Z 3.3.0 too3.4.0</span><span class="sxs-lookup"><span data-stu-id="ea0e2-107">From 3.3.0 too3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="ea0e2-108">Protokolů testování</span><span class="sxs-lookup"><span data-stu-id="ea0e2-108">Test logs</span></span>
<span data-ttu-id="ea0e2-109">Protokoly konzoly vyprodukované hello SDK teď může být povolena nebo zakázána nebo filtrovat.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="ea0e2-110">toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="ea0e2-111">Zdroje</span><span class="sxs-lookup"><span data-stu-id="ea0e2-111">Resources</span></span>
<span data-ttu-id="ea0e2-112">bylo vylepšeno Hello Reach překrytí.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-112">hello Reach overlay has been improved.</span></span> <span data-ttu-id="ea0e2-113">Je součástí prostředky pro balíček NuGet sady SDK hello.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-113">It is part of hello SDK NuGet package resources.</span></span>

<span data-ttu-id="ea0e2-114">Při upgradu toohello novou verzi hello SDK můžete zvolit, že jestli se mají tookeep existující soubory z hello překrytí složku vašich prostředků, nebo není:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-114">While upgrading toohello new version of hello SDK you can choose whether you want tookeep your existing files from hello overlay folder of your resources or not:</span></span>

* <span data-ttu-id="ea0e2-115">Pokud předchozí překrytí hello pracuje pro vás nebo jsou integrací hello `WebView` elementy ručně pak můžete rozhodnout tookeep, vaše stávající soubory, ji budou i nadále fungovat.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-115">If hello previous overlay is working for you or you are integrating hello `WebView` elements manually then you can decide tookeep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="ea0e2-116">Pokud chcete, aby tooupdate toohello nové překrytí, jenom nahradit hello celou `overlay` složky z vašich prostředků s novým hello z balíčku SDK hello (aplikace UWP: Po provedení upgradu hello můžete získat novou složku překrytí hello % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="ea0e2-116">If you want tooupdate toohello new overlay, just replace hello whole `overlay` folder from your resources with hello new one from hello SDK package (UWP apps: after hello upgrade, you can get hello new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="ea0e2-117">Pomocí nové překrytí hello přepíše všechny úpravy probíhají hello předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-117">Using hello new overlay will overwrite any customizations made on hello previous version.</span></span>
> 
> 

## <a name="from-320-too330"></a><span data-ttu-id="ea0e2-118">Z 3.2.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="ea0e2-118">From 3.2.0 too3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="ea0e2-119">Zdroje</span><span class="sxs-lookup"><span data-stu-id="ea0e2-119">Resources</span></span>
<span data-ttu-id="ea0e2-120">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-120">This step concerns customized resources only.</span></span> <span data-ttu-id="ea0e2-121">Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-121">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-too320"></a><span data-ttu-id="ea0e2-122">Z 3.1.0 too3.2.0</span><span class="sxs-lookup"><span data-stu-id="ea0e2-122">From 3.1.0 too3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="ea0e2-123">Zdroje</span><span class="sxs-lookup"><span data-stu-id="ea0e2-123">Resources</span></span>
<span data-ttu-id="ea0e2-124">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-124">This step concerns customized resources only.</span></span> <span data-ttu-id="ea0e2-125">Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-125">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="ea0e2-126">Integrace webové zobrazení</span><span class="sxs-lookup"><span data-stu-id="ea0e2-126">Webview integration</span></span>
<span data-ttu-id="ea0e2-127">V této verzi byly zavedeny velikostem některé vylepšení toomatch jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-127">Some improvements toomatch different device form factors were introduced in this version.</span></span> <span data-ttu-id="ea0e2-128">Ujistěte se, že vaše integrace webové zobrazení hello odpovídají hello následující:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-128">Make sure that your integration of hello webview match hello following:</span></span>

<span data-ttu-id="ea0e2-129">Ve vaší () stránky XAML:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="ea0e2-130">A v souboru přidružené .cs:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-130">And in your associated .cs file:</span></span>

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated toowithin a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

              #region tooimplement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update hello webview when hello app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update hello webview when hello app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure toodetach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are hello current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements toohello correct size.
              /// </summary>
              /// <param name="width">hello width of your current display.</param>
              /// <param name="height">hello height of your current display.</param>
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
              /// Handler that takes hello Windows.Current.SizeChanged and indicates that webviews have toobe resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes hello ApplicationView.VisibleBoundsChanged and indicates that webviews have toobe resized
              /// </summary>
              /// <param name="sender">hello related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-too300"></a><span data-ttu-id="ea0e2-131">Z 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="ea0e2-131">From 2.0.0 too3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="ea0e2-132">Zdroje</span><span class="sxs-lookup"><span data-stu-id="ea0e2-132">Resources</span></span>
<span data-ttu-id="ea0e2-133">Tento krok se týká jenom vlastní prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-133">This step concerns customized resources only.</span></span> <span data-ttu-id="ea0e2-134">Pokud jste upravili hello prostředky poskytované hello SDK (html, obrázky, překrytí), pak máte toobackup je před upgradem a použít znovu vlastní na upgradovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-134">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-too200"></a><span data-ttu-id="ea0e2-135">Z 1.1.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="ea0e2-135">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="ea0e2-136">Hello následující text popisuje, jak toomigrate integraci sady SDK z hello Capptain služby nabízených Capptain SAS do aplikace používá technologii Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-136">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ea0e2-137">Capptain a Mobile Engagement nejsou hello stejné služby a postup hello níže uvedené jenom označuje, jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-137">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="ea0e2-138">Migrace hello SDK v aplikaci hello není migrace dat ze sady Mobile Engagement pro toohello hello Capptain servery</span><span class="sxs-lookup"><span data-stu-id="ea0e2-138">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="ea0e2-139">Pokud provádíte migraci ze starší verze, prosím nejdřív najdete hello Capptain webu toomigrate too1.1.1 pak použít následující postup hello</span><span class="sxs-lookup"><span data-stu-id="ea0e2-139">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="ea0e2-140">Balíček Nuget</span><span class="sxs-lookup"><span data-stu-id="ea0e2-140">Nuget package</span></span>
<span data-ttu-id="ea0e2-141">Nahraďte **Capptain.WindowsPhone** podle **MicrosoftAzure.MobileEngagement** balíček Nuget.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="ea0e2-142">Použití Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="ea0e2-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="ea0e2-143">Hello SDK používá termín hello `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-143">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="ea0e2-144">Je třeba tooupdate vašeho projektu toomatch tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-144">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="ea0e2-145">Je nutné toouninstall váš aktuální Capptain balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-145">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="ea0e2-146">Zvažte, se odeberou všechny změny ve složce Capptain prostředky.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="ea0e2-147">Pokud chcete, aby tookeep tyto soubory pak proveďte jejich kopii.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-147">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="ea0e2-148">Potom nainstalujte balíček nuget Microsoft Azure Engagement nové hello na projektu.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-148">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="ea0e2-149">Najdete ho přímo na [nuget webu].</span><span class="sxs-lookup"><span data-stu-id="ea0e2-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="ea0e2-150">nebo sem index.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-150">or here index.</span></span> <span data-ttu-id="ea0e2-151">Tato akce nahradí všechny soubory prostředky používané Engagement a přidá nové tooyour Engagement DLL hello projektu odkazy.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-151">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="ea0e2-152">Tooclean máte odstraněním Capptain DLL odkazy odkazy projektu.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-152">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="ea0e2-153">Pokud to neuděláte, budou v konfliktu hello verzi Capptain a dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-153">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="ea0e2-154">Pokud jste upravili Capptain prostředky, zkopírujte původní soubory obsahu a vložit do nové soubory Engagement hello.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-154">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="ea0e2-155">Upozorňujeme, že soubory xaml a cs obsahují toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-155">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="ea0e2-156">Po dokončení těchto kroků stačí tooreplace staré odkazy Capptain podle hello nové Engagement odkazy.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-156">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="ea0e2-157">Všechny obory názvů Capptain mít toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-157">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="ea0e2-158">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="ea0e2-159">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="ea0e2-160">Všechny třídy Capptain, které obsahují "Capptain" by měly obsahovat "Engagement".</span><span class="sxs-lookup"><span data-stu-id="ea0e2-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="ea0e2-161">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="ea0e2-162">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="ea0e2-163">Pro soubory xaml Capptain obor názvů a atributy také změnit.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="ea0e2-164">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="ea0e2-165">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="ea0e2-166">Změny stránky překrytí</span><span class="sxs-lookup"><span data-stu-id="ea0e2-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ea0e2-167">Překrytí také změní.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-167">Overlay also changes.</span></span> <span data-ttu-id="ea0e2-168">Jeho nový obor názvů je `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="ea0e2-169">Má toobe používají v souborech xaml a cs.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-169">It has toobe used in both xaml and cs files.</span></span> <span data-ttu-id="ea0e2-170">Kromě toho `CapptainGrid` jmenuje toobe `EngagementGrid`, `capptain_notification_content` a `capptain_announcement_content` jsou pojmenované `engagement_notification_content` a `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-170">Moreover `CapptainGrid` is toobe named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="ea0e2-171">Pro překrytí:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="ea0e2-172">Stane se:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="ea0e2-173">Pro hello jiné prostředky, jako je Capptain obrázky a soubory HTML, Všimněte si, že také byly přejmenovat toouse "Engagement".</span><span class="sxs-lookup"><span data-stu-id="ea0e2-173">For hello other resources like Capptain pictures and HTML files, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="ea0e2-174">Deklarace projektu</span><span class="sxs-lookup"><span data-stu-id="ea0e2-174">Project declaration</span></span>
<span data-ttu-id="ea0e2-175">Na Package.appxmanifest `File Type Associations` byla aktualizována z:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="ea0e2-176">capptain\_dosáhnout\_obsahu tooengagement\_dosáhnout\_obsahu</span><span class="sxs-lookup"><span data-stu-id="ea0e2-176">capptain\_reach\_content tooengagement\_reach\_content</span></span>
* <span data-ttu-id="ea0e2-177">capptain\_protokolu\_souboru tooengagement\_protokolu\_souboru</span><span class="sxs-lookup"><span data-stu-id="ea0e2-177">capptain\_log\_file tooengagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="ea0e2-178">ID aplikace nebo klíč SDK</span><span class="sxs-lookup"><span data-stu-id="ea0e2-178">Application ID / SDK Key</span></span>
<span data-ttu-id="ea0e2-179">Zapojení používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-179">Engagement uses a connection string.</span></span> <span data-ttu-id="ea0e2-180">Nemáte toospecify ID aplikací a klíčem SDK s Mobile Engagement, stačí toospecify připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-180">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="ea0e2-181">Můžete ho nastavit tak v souboru EngagementConfiguration.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="ea0e2-182">Hello Engagement konfigurace může být nastavena v vaší `Resources\EngagementConfiguration.xml` souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-182">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="ea0e2-183">Upravte tento soubor toospecify:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-183">Edit this file toospecify:</span></span>

* <span data-ttu-id="ea0e2-184">Připojovací řetězec aplikací mezi značky `<connectionString>` a `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="ea0e2-185">Pokud chcete, aby ho za běhu místo toho můžete volat hello následující toospecify metoda před inicializací agenta hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-185">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="ea0e2-186">Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure Classic hello.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-186">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="ea0e2-187">Změna názvu položky</span><span class="sxs-lookup"><span data-stu-id="ea0e2-187">Items name change</span></span>
<span data-ttu-id="ea0e2-188">Všechny položky s názvem *capptain* byly pojmenovány *engagement*.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="ea0e2-189">Podobně jako pro *Capptain* příliš*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-189">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="ea0e2-190">Příklady běžně používané Capptain položek:</span><span class="sxs-lookup"><span data-stu-id="ea0e2-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="ea0e2-191">CapptainConfiguration nyní s názvem EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="ea0e2-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="ea0e2-192">Nyní s názvem EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="ea0e2-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="ea0e2-193">Nyní s názvem EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="ea0e2-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="ea0e2-194">Nyní s názvem EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="ea0e2-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="ea0e2-195">Nyní s názvem GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="ea0e2-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="ea0e2-196">Všimněte si, že přejmenovat také ovlivňuje přepsat metody.</span><span class="sxs-lookup"><span data-stu-id="ea0e2-196">Note that rename also affects overridden methods.</span></span>

