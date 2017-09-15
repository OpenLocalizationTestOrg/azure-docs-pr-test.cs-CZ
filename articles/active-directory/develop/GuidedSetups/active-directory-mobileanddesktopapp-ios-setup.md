---
title: "Azure AD v2 iOS Začínáme - instalace | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: d25353a61b2a60bff28aa0679d38110e77d19e64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="setting-up-your-ios-application"></a><span data-ttu-id="e97d0-103">Nastavení aplikace iOS</span><span class="sxs-lookup"><span data-stu-id="e97d0-103">Setting up your iOS application</span></span>

<span data-ttu-id="e97d0-104">Tato část obsahuje podrobné pokyny pro vytvoření nového projektu do ukazují, jak integrovat aplikace pro iOS (Swift) s *přihlášení se společností Microsoft* , může se dotázat webovým rozhraním API, které vyžadují token.</span><span class="sxs-lookup"><span data-stu-id="e97d0-104">This section provides step-by-step instructions for how to create a new project to demonstrate how to integrate an iOS application (Swift) with *Sign-In with Microsoft* so it can query Web APIs that require a token.</span></span>

> <span data-ttu-id="e97d0-105">Stáhněte si tento ukázkový projekt XCode místo dávají přednost?</span><span class="sxs-lookup"><span data-stu-id="e97d0-105">Prefer to download this sample's XCode project instead?</span></span> <span data-ttu-id="e97d0-106">[Stažení projektu](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) a pokračujte [krok konfigurace](#create-an-application-express) před provedením konfigurace ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="e97d0-106">[Download a project](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) and skip to the [Configuration step](#create-an-application-express) to configure the code sample before executing.</span></span>


## <a name="install-carthage-to-download-and-build-msal"></a><span data-ttu-id="e97d0-107">Nainstalujte Carthage ke stažení a sestavení MSAL</span><span class="sxs-lookup"><span data-stu-id="e97d0-107">Install Carthage to download and build MSAL</span></span>
<span data-ttu-id="e97d0-108">Správce balíčků Carthage se používá během období preview MSAL – integruje se s XCode při zachování schopnost Microsoft provést změny do knihovny.</span><span class="sxs-lookup"><span data-stu-id="e97d0-108">Carthage package manager is used during the preview period of MSAL – it integrates with XCode while maintaining the ability for Microsoft to make changes to the library.</span></span>

- <span data-ttu-id="e97d0-109">Stáhněte a nainstalujte nejnovější verzi Carthage [sem](https://github.com/Carthage/Carthage/releases "Carthage adresa URL pro stahování")</span><span class="sxs-lookup"><span data-stu-id="e97d0-109">Download and install the latest release of Carthage [here](https://github.com/Carthage/Carthage/releases "Carthage download URL")</span></span>

## <a name="creating-your-application"></a><span data-ttu-id="e97d0-110">Vytvoření vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="e97d0-110">Creating your application</span></span>

1.  <span data-ttu-id="e97d0-111">Otevřete Xcode a vyberte`Create a new Xcode project`</span><span class="sxs-lookup"><span data-stu-id="e97d0-111">Open Xcode and select `Create a new Xcode project`</span></span>
2.  <span data-ttu-id="e97d0-112">Vyberte `iOS`  >  `Single view Application` a klikněte na tlačítko *další*</span><span class="sxs-lookup"><span data-stu-id="e97d0-112">Select `iOS` > `Single view Application` and click *Next*</span></span>
3.  <span data-ttu-id="e97d0-113">Zadejte název produktu a klikněte na *další*</span><span class="sxs-lookup"><span data-stu-id="e97d0-113">Give a product name and click *Next*</span></span>
4.  <span data-ttu-id="e97d0-114">Vyberte složku pro vytvoření aplikace a klikněte na tlačítko *vytvořit*</span><span class="sxs-lookup"><span data-stu-id="e97d0-114">Select a folder to create your app and click *Create*</span></span>

## <a name="build-the-msal-framework"></a><span data-ttu-id="e97d0-115">Sestavení rozhraní MSAL</span><span class="sxs-lookup"><span data-stu-id="e97d0-115">Build the MSAL Framework</span></span>

<span data-ttu-id="e97d0-116">Postupujte podle pokynů pro vyžádání obsahu a následně vytvořit nejnovější verze knihoven MSAL pomocí Carthage:</span><span class="sxs-lookup"><span data-stu-id="e97d0-116">Follow the instructions below to pull and then build the latest version of MSAL libraries using Carthage:</span></span>

1.  <span data-ttu-id="e97d0-117">Otevřete terminál bash a přejděte do kořenové složky aplikace</span><span class="sxs-lookup"><span data-stu-id="e97d0-117">Open the bash terminal and go to the App’s root folder</span></span>
2.  <span data-ttu-id="e97d0-118">Kopírování níže a vkládání v terminálu bash k vytvoření souboru 'Cartfile':</span><span class="sxs-lookup"><span data-stu-id="e97d0-118">Copy the below and paste in the bash terminal to create a ‘Cartfile’ file:</span></span>

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
<span data-ttu-id="e97d0-119">Zkopírujte a vložte níže.</span><span class="sxs-lookup"><span data-stu-id="e97d0-119">Copy and paste the below.</span></span> <span data-ttu-id="e97d0-120">Tento příkaz načte závislosti do složky Carthage/rezervace, a poté vytvoří MSAL knihovny:</span><span class="sxs-lookup"><span data-stu-id="e97d0-120">This command fetches dependencies into a Carthage/Checkouts folder, then builds the MSAL library:</span></span>
</li>
</ol>

```bash
carthage update
```

> <span data-ttu-id="e97d0-121">Výše uvedené procesu se používá ke stažení a sestavení knihovny ověřování společnosti Microsoft (MSAL).</span><span class="sxs-lookup"><span data-stu-id="e97d0-121">The process above is used to download and build the Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="e97d0-122">MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci tokeny uživatel používá pro přístup k rozhraní API, které jsou chráněné službou Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="e97d0-122">MSAL handles acquiring, caching and refreshing user tokens used to access APIs protected by the Azure Active Directory v2.</span></span>

## <a name="add-the-msal-framework-to-your-application"></a><span data-ttu-id="e97d0-123">Přidání rozhraní MSAL do aplikace</span><span class="sxs-lookup"><span data-stu-id="e97d0-123">Add the MSAL framework to your application</span></span>
1.  <span data-ttu-id="e97d0-124">V Xcode otevřete `General` karta</span><span class="sxs-lookup"><span data-stu-id="e97d0-124">In Xcode, open the `General` tab</span></span>
2.  <span data-ttu-id="e97d0-125">Přejděte na `Linked Frameworks and Libraries` a klikněte na`+`</span><span class="sxs-lookup"><span data-stu-id="e97d0-125">Go to the `Linked Frameworks and Libraries` section and click `+`</span></span>
3.  <span data-ttu-id="e97d0-126">Vyberte `Add other…`</span><span class="sxs-lookup"><span data-stu-id="e97d0-126">Select `Add other…`</span></span>
4.  <span data-ttu-id="e97d0-127">Vyberte: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` a klikněte na tlačítko *otevřete*.</span><span class="sxs-lookup"><span data-stu-id="e97d0-127">Select: `Carthage` > `Build` > `iOS` > `MSAL.framework` and click *Open*.</span></span> <span data-ttu-id="e97d0-128">Měli byste vidět `MSAL.framework` přidán do seznamu.</span><span class="sxs-lookup"><span data-stu-id="e97d0-128">You should see `MSAL.framework` added to the list.</span></span>
5.  <span data-ttu-id="e97d0-129">Přejděte na `Build Phases` a klikněte na `+` ikonu, zvolte`New Run Script Phase`</span><span class="sxs-lookup"><span data-stu-id="e97d0-129">Go to `Build Phases` tab, and click `+` icon, choose `New Run Script Phase`</span></span>
6.  <span data-ttu-id="e97d0-130">Přidejte následující obsah *skript oblasti*:</span><span class="sxs-lookup"><span data-stu-id="e97d0-130">Add the following contents to the *script area*:</span></span>

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="e97d0-131">Přidejte následující <code>Input Files</code> kliknutím <code>+</code>:</span><span class="sxs-lookup"><span data-stu-id="e97d0-131">Add the following to <code>Input Files</code> by clicking <code>+</code>:</span></span>
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a><span data-ttu-id="e97d0-132">Vytvoření uživatelského rozhraní aplikace</span><span class="sxs-lookup"><span data-stu-id="e97d0-132">Creating your application’s UI</span></span>
<span data-ttu-id="e97d0-133">Soubor Main.storyboard by měl automaticky vytvoří jako součást vaše šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="e97d0-133">A Main.storyboard file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="e97d0-134">Postupujte podle pokynů k vytvoření aplikace uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="e97d0-134">Follow the instructions below to create the app UI:</span></span>

1.  <span data-ttu-id="e97d0-135">CTRL + klikněte na tlačítko `Main.storyboard` zprovoznit v kontextové nabídce a potom klikněte na:`Open As` > `Source Code`</span><span class="sxs-lookup"><span data-stu-id="e97d0-135">Control+click `Main.storyboard` to bring up the contextual menu, and then click: `Open As` > `Source Code`</span></span>
2.  <span data-ttu-id="e97d0-136">Nahraďte `<scenes>` uzel s následující kód:</span><span class="sxs-lookup"><span data-stu-id="e97d0-136">Replace the `<scenes>` node with the code below:</span></span>

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign-Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
