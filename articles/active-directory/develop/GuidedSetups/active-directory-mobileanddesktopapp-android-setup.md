---
title: "aaaAzure AD v2 Android Začínáme - instalace | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API Android Asie"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: df2670d6d35b7a9a81158d4d7eb190540ca9c695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a>Nastavení projektu

> Dáváte přednost toodownload tento ukázkový projekt Android Studio místo? [Stažení projektu](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.


### <a name="create-a-new-project"></a>Vytvoření nového projektu 
1.  Otevřete Android Studio a přejděte do:`File` > `New` > `New Project`
2.  Název aplikace a klikněte na tlačítko`Next`
3.  Ujistěte se, že tooselect *21 rozhraní API nebo novější (Android 5.0)* a klikněte na tlačítko`Next`
4.  Nechte `Empty Activity`, klikněte na tlačítko `Next`, pak`Finish`


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Přidání projektu tooyour hello Microsoft ověřování knihovny (MSAL)
1.  V nástroji Android Studio přejděte do:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Kopírování a vložení hello následující kód pod `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>O tomto balíčku

balíček Hello výše nainstaluje hello Microsoft ověřování knihovny (MSAL). MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci uživatele použití tokenů tooaccess chráněné službou Azure Active Directory v2 koncový bod rozhraní API.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Vytvoření uživatelského rozhraní aplikace

1.  Otevřete: `activity_main.xml` v části`res` > `layout`
2.  Změna rozložení aktivity hello z `android.support.constraint.ConstraintLayout` či jiné příliš`LinearLayout`
3.  Přidat `android:orientation="vertical"` vlastnost příliš`LinearLayout` uzlu
4.  Kopírování a vložení hello následující kód do hello `LinearLayout` uzlu, nahraďte obsah aktuální hello:

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

