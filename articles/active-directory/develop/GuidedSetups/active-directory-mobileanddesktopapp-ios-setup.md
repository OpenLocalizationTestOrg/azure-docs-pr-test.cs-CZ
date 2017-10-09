---
title: "iOS v2 aaaAzure AD Začínáme - instalace | Microsoft Docs"
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
ms.openlocfilehash: 62c4ee9a2d4ccaec780bee09fb4bc34cff2eb6df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-ios-application"></a>Nastavení aplikace iOS

Tato část obsahuje podrobné pokyny, jak toocreate nový projekt toodemonstrate jak toointegrate aplikace pro iOS (Swift) s *přihlášení se společností Microsoft* , může se dotázat webovým rozhraním API, které vyžadují token.

> Dáváte přednost toodownload tento ukázkový projekt XCode místo? [Stažení projektu](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.


## <a name="install-carthage-toodownload-and-build-msal"></a>Nainstalujte Carthage toodownload a sestavení MSAL
Správce balíčků Carthage se používá během období preview hello MSAL – integruje se s XCode při zachování schopnosti hello Microsoft toomake změny toohello knihovny.

- Stáhněte a nainstalujte nejnovější verzi Carthage hello [sem](https://github.com/Carthage/Carthage/releases "Carthage adresa URL pro stahování")

## <a name="creating-your-application"></a>Vytvoření vaší aplikace

1.  Otevřete Xcode a vyberte`Create a new Xcode project`
2.  Vyberte `iOS`  >  `Single view Application` a klikněte na tlačítko *další*
3.  Zadejte název produktu a klikněte na *další*
4.  Vyberte složku toocreate vaší aplikace a klikněte na *vytvořit*

## <a name="build-hello-msal-framework"></a>Sestavení hello MSAL Framework

Postupujte podle pokynů hello níže toopull a následně vytvořit hello nejnovější verzi knihovny MSAL pomocí Carthage:

1.  Otevřete terminál hello bash a přejděte toohello aplikace kořenové složky
2.  Hello níže zkopírujte a vložte hello bash terminálu toocreate soubor 'Cartfile':

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Zkopírujte a vložte hello níže. Tento příkaz načte závislosti do složky Carthage/rezervace, a poté vytvoří hello MSAL knihovny:
</li>
</ol>

```bash
carthage update
```

> proces Hello výše je použité toodownload a sestavení hello Microsoft ověřování knihovny (MSAL). MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci tooaccess tokeny použít uživatelské rozhraní API chráněn hello v2 Azure Active Directory.

## <a name="add-hello-msal-framework-tooyour-application"></a>Přidání hello MSAL framework tooyour aplikace
1.  V Xcode otevřete hello `General` karta
2.  Přejděte toohello `Linked Frameworks and Libraries` a klikněte na`+`
3.  Vyberte `Add other…`
4.  Vyberte: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` a klikněte na tlačítko *otevřete*. Měli byste vidět `MSAL.framework` přidat toohello seznamu.
5.  Přejděte příliš`Build Phases` a klikněte na `+` ikonu, zvolte`New Run Script Phase`
6.  Přidejte následující obsah toohello hello *skript oblasti*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Přidejte následující hello příliš<code>Input Files</code> kliknutím <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Vytvoření uživatelského rozhraní aplikace
Soubor Main.storyboard by měl automaticky vytvoří jako součást vaše šablona projektu. Postupujte podle pokynů hello níže toocreate hello aplikace uživatelského rozhraní:

1.  CTRL + klikněte na tlačítko `Main.storyboard` toobring až hello kontextové nabídky a potom klikněte na:`Open As` > `Source Code`
2.  Nahraďte hello `<scenes>` uzel s hello kódu níže:

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
