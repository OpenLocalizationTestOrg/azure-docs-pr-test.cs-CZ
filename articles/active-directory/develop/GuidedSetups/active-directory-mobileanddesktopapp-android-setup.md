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
## <a name="set-up-your-project"></a><span data-ttu-id="5f0a6-103">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="5f0a6-103">Set up your project</span></span>

> <span data-ttu-id="5f0a6-104">Dáváte přednost toodownload tento ukázkový projekt Android Studio místo?</span><span class="sxs-lookup"><span data-stu-id="5f0a6-104">Prefer toodownload this sample's Android Studio project instead?</span></span> <span data-ttu-id="5f0a6-105">[Stažení projektu](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.</span><span class="sxs-lookup"><span data-stu-id="5f0a6-105">[Download a project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing    .</span></span>


### <a name="create-a-new-project"></a><span data-ttu-id="5f0a6-106">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="5f0a6-106">Create a new project</span></span> 
1.  <span data-ttu-id="5f0a6-107">Otevřete Android Studio a přejděte do:`File` > `New` > `New Project`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-107">Open Android Studio, go to: `File` > `New` > `New Project`</span></span>
2.  <span data-ttu-id="5f0a6-108">Název aplikace a klikněte na tlačítko`Next`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-108">Name your application and click `Next`</span></span>
3.  <span data-ttu-id="5f0a6-109">Ujistěte se, že tooselect *21 rozhraní API nebo novější (Android 5.0)* a klikněte na tlačítko`Next`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-109">Make sure tooselect *API 21 or newer (Android 5.0)* and click `Next`</span></span>
4.  <span data-ttu-id="5f0a6-110">Nechte `Empty Activity`, klikněte na tlačítko `Next`, pak`Finish`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-110">Leave `Empty Activity`, click `Next`, then `Finish`</span></span>


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="5f0a6-111">Přidání projektu tooyour hello Microsoft ověřování knihovny (MSAL)</span><span class="sxs-lookup"><span data-stu-id="5f0a6-111">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1.  <span data-ttu-id="5f0a6-112">V nástroji Android Studio přejděte do:`Gradle Scripts` > `build.gradle (Module: app)`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-112">In Android Studio, go to: `Gradle Scripts` > `build.gradle (Module: app)`</span></span>
2.  <span data-ttu-id="5f0a6-113">Kopírování a vložení hello následující kód pod `Dependencies`:</span><span class="sxs-lookup"><span data-stu-id="5f0a6-113">Copy and paste hello following code under `Dependencies`:</span></span>

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a><span data-ttu-id="5f0a6-114">O tomto balíčku</span><span class="sxs-lookup"><span data-stu-id="5f0a6-114">About this package</span></span>

<span data-ttu-id="5f0a6-115">balíček Hello výše nainstaluje hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="5f0a6-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="5f0a6-116">MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci uživatele použití tokenů tooaccess chráněné službou Azure Active Directory v2 koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5f0a6-116">MSAL handles acquiring, caching and refreshing user tokens used tooaccess APIs protected by Azure Active Directory v2 endpoint.</span></span>
<!--end-collapse-->

## <a name="create-your-applications-ui"></a><span data-ttu-id="5f0a6-117">Vytvoření uživatelského rozhraní aplikace</span><span class="sxs-lookup"><span data-stu-id="5f0a6-117">Create your application’s UI</span></span>

1.  <span data-ttu-id="5f0a6-118">Otevřete: `activity_main.xml` v části`res` > `layout`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-118">Open: `activity_main.xml` under `res` > `layout`</span></span>
2.  <span data-ttu-id="5f0a6-119">Změna rozložení aktivity hello z `android.support.constraint.ConstraintLayout` či jiné příliš`LinearLayout`</span><span class="sxs-lookup"><span data-stu-id="5f0a6-119">Change hello activity layout from `android.support.constraint.ConstraintLayout` or other too`LinearLayout`</span></span>
3.  <span data-ttu-id="5f0a6-120">Přidat `android:orientation="vertical"` vlastnost příliš`LinearLayout` uzlu</span><span class="sxs-lookup"><span data-stu-id="5f0a6-120">Add `android:orientation="vertical"` property too`LinearLayout` node</span></span>
4.  <span data-ttu-id="5f0a6-121">Kopírování a vložení hello následující kód do hello `LinearLayout` uzlu, nahraďte obsah aktuální hello:</span><span class="sxs-lookup"><span data-stu-id="5f0a6-121">Copy and paste hello following code into hello `LinearLayout` node, replacing hello current content:</span></span>

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

