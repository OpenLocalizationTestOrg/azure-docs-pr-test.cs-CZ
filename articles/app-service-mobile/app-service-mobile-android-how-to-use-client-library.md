---
title: aaaHow toouse hello Azure Mobile Apps SDK pro Android | Microsoft Docs
description: Jak toouse hello Azure Mobile Apps SDK pro Android
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: glenga
ms.openlocfilehash: 56eb73c4e1703d69877be499a09fc2130f1d68e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a><span data-ttu-id="e3624-103">Jak toouse hello Azure Mobile Apps SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="e3624-103">How toouse hello Azure Mobile Apps SDK for Android</span></span>

<span data-ttu-id="e3624-104">Tento průvodce vám ukáže, jak toouse hello Android klienta SDK pro Mobile Apps tooimplement běžné scénáře, jako například:</span><span class="sxs-lookup"><span data-stu-id="e3624-104">This guide shows you how toouse hello Android client SDK for Mobile Apps tooimplement common scenarios, such as:</span></span>

* <span data-ttu-id="e3624-105">Dotazování na data (vložení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="e3624-105">Querying for data (inserting, updating, and deleting).</span></span>
* <span data-ttu-id="e3624-106">Ověřování.</span><span class="sxs-lookup"><span data-stu-id="e3624-106">Authentication.</span></span>
* <span data-ttu-id="e3624-107">Zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="e3624-107">Handling errors.</span></span>
* <span data-ttu-id="e3624-108">Přizpůsobení hello klienta.</span><span class="sxs-lookup"><span data-stu-id="e3624-108">Customizing hello client.</span></span>

<span data-ttu-id="e3624-109">Tato příručka se zaměřuje na hello klientské sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="e3624-109">This guide focuses on hello client-side Android SDK.</span></span>  <span data-ttu-id="e3624-110">Další informace o hello serverové sady SDK pro Mobile Apps naleznete v tématu toolearn [pracovat s .NET back-end SDK] [ 10] nebo [jak toouse hello back-end Node.js SDK] [ 11].</span><span class="sxs-lookup"><span data-stu-id="e3624-110">toolearn more about hello server-side SDKs for Mobile Apps, see [Work with .NET backend SDK][10] or [How toouse hello Node.js backend SDK][11].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="e3624-111">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e3624-111">Reference Documentation</span></span>

<span data-ttu-id="e3624-112">Můžete najít hello [referenční dokumentace rozhraní API Javadocs] [ 12] pro Android klientské knihovny hello na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e3624-112">You can find hello [Javadocs API reference][12] for hello Android client library on GitHub.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e3624-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="e3624-113">Supported Platforms</span></span>

<span data-ttu-id="e3624-114">Hello Azure Mobile Apps SDK pro Android podporuje rozhraní API úrovně 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát) pro telefon i tablet velikostem.</span><span class="sxs-lookup"><span data-stu-id="e3624-114">hello Azure Mobile Apps SDK for Android supports API levels 19 through 24 (KitKat through Nougat) for phone and tablet form factors.</span></span>  <span data-ttu-id="e3624-115">Ověřování, zejména využívá běžné webové framework přístup toogather přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e3624-115">Authentication, in particular, utilizes a common web framework approach toogather credentials.</span></span>  <span data-ttu-id="e3624-116">Ověřování serveru toku nefunguje s malé formuláře Multi-Factor zařízení, jako jsou sleduje.</span><span class="sxs-lookup"><span data-stu-id="e3624-116">Server-flow authentication does not work with small form factor devices such as watches.</span></span>

## <a name="setup-and-prerequisites"></a><span data-ttu-id="e3624-117">Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="e3624-117">Setup and Prerequisites</span></span>

<span data-ttu-id="e3624-118">Dokončení hello [využít postup rychlého spuštění Mobile Apps](app-service-mobile-android-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3624-118">Complete hello [Mobile Apps quickstart](app-service-mobile-android-get-started.md) tutorial.</span></span>  <span data-ttu-id="e3624-119">Tato úloha zajistí, že byly splněny všechny požadavky pro Azure Mobile Apps pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="e3624-119">This task ensures all pre-requisites for developing Azure Mobile Apps have been met.</span></span>  <span data-ttu-id="e3624-120">Hello rychlý start také vám pomůže nakonfigurovat svůj účet a vytvoření vaší první back-endu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-120">hello Quickstart also helps you configure your account and create your first Mobile App backend.</span></span>

<span data-ttu-id="e3624-121">Pokud se rozhodnete není toocomplete hello rychlý úvodní kurz, dokončete hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e3624-121">If you decide not toocomplete hello Quickstart tutorial, complete hello following tasks:</span></span>

* <span data-ttu-id="e3624-122">[Vytvořte back-end mobilní aplikace] [ 13] toouse s vaší aplikací pro Android.</span><span class="sxs-lookup"><span data-stu-id="e3624-122">[create a Mobile App backend][13] toouse with your Android app.</span></span>
* <span data-ttu-id="e3624-123">V nástroji Android Studio [aktualizace hello Gradle sestavení souborů](#gradle-build).</span><span class="sxs-lookup"><span data-stu-id="e3624-123">In Android Studio, [update hello Gradle build files](#gradle-build).</span></span>
* <span data-ttu-id="e3624-124">[Povolit oprávnění internet](#enable-internet).</span><span class="sxs-lookup"><span data-stu-id="e3624-124">[Enable internet permission](#enable-internet).</span></span>

### <span data-ttu-id="e3624-125"><a name="gradle-build"></a>Aktualizace hello Gradle vytvoření souboru</span><span class="sxs-lookup"><span data-stu-id="e3624-125"><a name="gradle-build"></a>Update hello Gradle build file</span></span>

<span data-ttu-id="e3624-126">Obě změnit **build.gradle** soubory:</span><span class="sxs-lookup"><span data-stu-id="e3624-126">Change both **build.gradle** files:</span></span>

1. <span data-ttu-id="e3624-127">Přidejte tento kód toohello *projektu* úroveň **build.gradle** souboru uvnitř hello *buildscript* značky:</span><span class="sxs-lookup"><span data-stu-id="e3624-127">Add this code toohello *Project* level **build.gradle** file inside hello *buildscript* tag:</span></span>

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. <span data-ttu-id="e3624-128">Přidejte tento kód toohello *modulu aplikace* úroveň **build.gradle** souboru uvnitř hello *závislosti* značky:</span><span class="sxs-lookup"><span data-stu-id="e3624-128">Add this code toohello *Module app* level **build.gradle** file inside hello *dependencies* tag:</span></span>

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    <span data-ttu-id="e3624-129">Aktuálně hello nejnovější verze je 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="e3624-129">Currently hello latest version is 3.3.0.</span></span> <span data-ttu-id="e3624-130">Hello podporované verze jsou uvedeny [na panelu koše][14].</span><span class="sxs-lookup"><span data-stu-id="e3624-130">hello supported versions are listed [on bintray][14].</span></span>

### <span data-ttu-id="e3624-131"><a name="enable-internet"></a>Povolit oprávnění internet</span><span class="sxs-lookup"><span data-stu-id="e3624-131"><a name="enable-internet"></a>Enable internet permission</span></span>

<span data-ttu-id="e3624-132">tooaccess Azure, aplikace musí mít povolené oprávnění INTERNET hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-132">tooaccess Azure, your app must have hello INTERNET permission enabled.</span></span> <span data-ttu-id="e3624-133">Pokud ještě není povolené, přidejte následující řádek kódu tooyour hello **AndroidManifest.xml** souboru:</span><span class="sxs-lookup"><span data-stu-id="e3624-133">If it's not already enabled, add hello following line of code tooyour **AndroidManifest.xml** file:</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a><span data-ttu-id="e3624-134">Umožňuje vytvořit připojení klienta</span><span class="sxs-lookup"><span data-stu-id="e3624-134">Create a Client Connection</span></span>

<span data-ttu-id="e3624-135">Azure Mobile Apps poskytuje čtyři funkce tooyour mobilních aplikací:</span><span class="sxs-lookup"><span data-stu-id="e3624-135">Azure Mobile Apps provides four functions tooyour mobile application:</span></span>

* <span data-ttu-id="e3624-136">Přístup k datům a Offline synchronizace s služby mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="e3624-136">Data Access and Offline Synchronization with an Azure Mobile Apps Service.</span></span>
* <span data-ttu-id="e3624-137">Volání rozhraní API vlastní napsané pomocí hello Azure Mobile Apps Server SDK.</span><span class="sxs-lookup"><span data-stu-id="e3624-137">Call Custom APIs written with hello Azure Mobile Apps Server SDK.</span></span>
* <span data-ttu-id="e3624-138">Ověřování pomocí služby Azure App Service ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="e3624-138">Authentication with Azure App Service Authentication and Authorization.</span></span>
* <span data-ttu-id="e3624-139">Registrace nabízených oznámení v Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="e3624-139">Push Notification Registration with Notification Hubs.</span></span>

<span data-ttu-id="e3624-140">Každá z těchto funkcí vyžaduje nejprve vytvoření `MobileServiceClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="e3624-140">Each of these functions first requires that you create a `MobileServiceClient` object.</span></span>  <span data-ttu-id="e3624-141">Pouze jeden `MobileServiceClient` objekt by měl být vytvořen v rámci vašeho mobilního klienta (to znamená, musí být Singleton vzor).</span><span class="sxs-lookup"><span data-stu-id="e3624-141">Only one `MobileServiceClient` object should be created within your mobile client (that is, it should be a Singleton pattern).</span></span>  <span data-ttu-id="e3624-142">toocreate `MobileServiceClient` objektu:</span><span class="sxs-lookup"><span data-stu-id="e3624-142">toocreate a `MobileServiceClient` object:</span></span>

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

<span data-ttu-id="e3624-143">Hello `<MobileAppUrl>` je řetězec nebo objekt adresy URL, která bodů mobilního back-endu tooyour.</span><span class="sxs-lookup"><span data-stu-id="e3624-143">hello `<MobileAppUrl>` is either a string or a URL object that points tooyour mobile backend.</span></span>  <span data-ttu-id="e3624-144">Pokud používáte Azure App Service toohost mobilního back-endu, pak se ujistěte, použijte zabezpečené hello `https://` verze hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e3624-144">If you are using Azure App Service toohost your mobile backend, then ensure you use hello secure `https://` version of hello URL.</span></span>

<span data-ttu-id="e3624-145">Hello klient také vyžaduje přístup toohello aktivity nebo kontextu - hello `this` parametr v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-145">hello client also requires access toohello Activity or Context - hello `this` parameter in hello example.</span></span>  <span data-ttu-id="e3624-146">měly by během hello proběhnout Hello konstrukce MobileServiceClient `onCreate()` metoda hello aktivity odkazuje v hello `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="e3624-146">hello MobileServiceClient construction should happen within hello `onCreate()` method of hello Activity referenced in hello `AndroidManifest.xml` file.</span></span>

<span data-ttu-id="e3624-147">Jako osvědčený postup by měl abstraktní komunikace serveru do vlastní třídy (singleton-pattern).</span><span class="sxs-lookup"><span data-stu-id="e3624-147">As a best practice, you should abstract server communication into its own (singleton-pattern) class.</span></span>  <span data-ttu-id="e3624-148">V takovém případě by měla předávat hello aktivity v rámci hello konstruktor tooappropriately konfigurace služby hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-148">In this case, you should pass hello Activity within hello constructor tooappropriately configure hello service.</span></span>  <span data-ttu-id="e3624-149">Například:</span><span class="sxs-lookup"><span data-stu-id="e3624-149">For example:</span></span>

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

<span data-ttu-id="e3624-150">Nyní můžete volat `AzureServiceAdapter.Initialize(this);` v hello `onCreate()` metoda hlavní činnosti.</span><span class="sxs-lookup"><span data-stu-id="e3624-150">You can now call `AzureServiceAdapter.Initialize(this);` in hello `onCreate()` method of your main activity.</span></span>  <span data-ttu-id="e3624-151">Použít jiné metody, kteří potřebují přístup toohello klienta `AzureServiceAdapter.getInstance();` tooobtain adaptérem odkaz toohello služby.</span><span class="sxs-lookup"><span data-stu-id="e3624-151">Any other methods needing access toohello client use `AzureServiceAdapter.getInstance();` tooobtain a reference toohello service adapter.</span></span>

## <a name="data-operations"></a><span data-ttu-id="e3624-152">Operace s daty</span><span class="sxs-lookup"><span data-stu-id="e3624-152">Data Operations</span></span>

<span data-ttu-id="e3624-153">Hello jádrem hello Azure Mobile Apps SDK je toodata tooprovide přístup na back-end mobilní aplikace hello uložená v SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="e3624-153">hello core of hello Azure Mobile Apps SDK is tooprovide access toodata stored within SQL Azure on hello Mobile App backend.</span></span>  <span data-ttu-id="e3624-154">Můžete přístup k těmto datům pomocí silného typu třídy (doporučeno) nebo netypová dotazy (nedoporučuje se).</span><span class="sxs-lookup"><span data-stu-id="e3624-154">You can access this data using strongly typed classes (preferred) or untyped queries (not recommended).</span></span>  <span data-ttu-id="e3624-155">hromadné Hello této části se zabývá pomocí třídy silného typu.</span><span class="sxs-lookup"><span data-stu-id="e3624-155">hello bulk of this section deals with using strongly typed classes.</span></span>

### <a name="define-client-data-classes"></a><span data-ttu-id="e3624-156">Definování datových tříd, klienta</span><span class="sxs-lookup"><span data-stu-id="e3624-156">Define client data classes</span></span>

<span data-ttu-id="e3624-157">tooaccess data z tabulek SQL Azure, definovat klienta datových tříd, které odpovídají toohello tabulek v back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-157">tooaccess data from SQL Azure tables, define client data classes that correspond toohello tables in hello Mobile App backend.</span></span> <span data-ttu-id="e3624-158">Příklady v tomto tématu předpokládat tabulku s názvem **MyDataTable**, který má hello následující sloupce:</span><span class="sxs-lookup"><span data-stu-id="e3624-158">Examples in this topic assume a table named **MyDataTable**, which has hello following columns:</span></span>

* <span data-ttu-id="e3624-159">id</span><span class="sxs-lookup"><span data-stu-id="e3624-159">id</span></span>
* <span data-ttu-id="e3624-160">Text</span><span class="sxs-lookup"><span data-stu-id="e3624-160">text</span></span>
* <span data-ttu-id="e3624-161">Dokončení</span><span class="sxs-lookup"><span data-stu-id="e3624-161">complete</span></span>

<span data-ttu-id="e3624-162">Hello odpovídající typu objektu na straně klienta se nachází v souboru s názvem **MyDataTable.java**:</span><span class="sxs-lookup"><span data-stu-id="e3624-162">hello corresponding typed client-side object resides in a file called **MyDataTable.java**:</span></span>

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

<span data-ttu-id="e3624-163">Přidejte metody getter a setter metody pro každé pole, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="e3624-163">Add getter and setter methods for each field that you add.</span></span>  <span data-ttu-id="e3624-164">Pokud SQL Azure tabulka obsahuje více sloupců, měli byste přidat hello odpovídající pole toothis třídy.</span><span class="sxs-lookup"><span data-stu-id="e3624-164">If your SQL Azure table contains more columns, you would add hello corresponding fields toothis class.</span></span>  <span data-ttu-id="e3624-165">Například, pokud hello DTO sloupec s prioritou celé číslo bylo (objekt přenosu dat) a pak může přidejte toto pole, společně s její metody getter a setter metody:</span><span class="sxs-lookup"><span data-stu-id="e3624-165">For example, if hello DTO (data transfer object) had an integer Priority column, then you might add this field, along with its getter and setter methods:</span></span>

```java
private Integer priority;

/**
* Returns hello item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets hello item priority
*
* @param priority
*            priority tooset
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

<span data-ttu-id="e3624-166">toolearn jak zjistit, toocreate dalších tabulek v váš back-end mobilní aplikace [postupy: definování řadič tabulky] [ 15] (.NET back-end) nebo [definovat tabulky pomocí dynamické schématu] [ 16] (Back-end Node.js).</span><span class="sxs-lookup"><span data-stu-id="e3624-166">toolearn how toocreate additional tables in your Mobile Apps backend, see [How to: Define a table controller][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js backend).</span></span>

<span data-ttu-id="e3624-167">Tabulce back-end Azure Mobile Apps definuje pět speciální pole, čtyři, které jsou k dispozici tooclients:</span><span class="sxs-lookup"><span data-stu-id="e3624-167">An Azure Mobile Apps backend table defines five special fields, four of which are available tooclients:</span></span>

* <span data-ttu-id="e3624-168">`String id`: hello globálně jedinečné ID pro záznam hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-168">`String id`: hello globally unique ID for hello record.</span></span>  <span data-ttu-id="e3624-169">Jako osvědčený postup, ujistěte se, hello id hello řetězcovou reprezentaci [UUID] [ 17] objektu.</span><span class="sxs-lookup"><span data-stu-id="e3624-169">As a best practice, make hello id hello String representation of a [UUID][17] object.</span></span>
* <span data-ttu-id="e3624-170">`DateTimeOffset updatedAt`: hello datum a čas poslední aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-170">`DateTimeOffset updatedAt`: hello date/time of hello last update.</span></span>  <span data-ttu-id="e3624-171">Hello updatedAt pole je nastaven serverem hello a musí být nastavena nikdy váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="e3624-171">hello updatedAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="e3624-172">`DateTimeOffset createdAt`: hello datum a čas byl vytvořen tento objekt hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-172">`DateTimeOffset createdAt`: hello date/time that hello object was created.</span></span>  <span data-ttu-id="e3624-173">Hello createdAt pole je nastaven serverem hello a musí být nastavena nikdy váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="e3624-173">hello createdAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="e3624-174">`byte[] version`: Obvykle vyjádřený jako řetězec, hello verze je také nastavit server hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-174">`byte[] version`: Normally represented as a string, hello version is also set by hello server.</span></span>
* <span data-ttu-id="e3624-175">`boolean deleted`: Označuje, že záznam hello má byla odstraněna ale ještě není vyprázdní.</span><span class="sxs-lookup"><span data-stu-id="e3624-175">`boolean deleted`: Indicates that hello record has been deleted but not purged yet.</span></span>  <span data-ttu-id="e3624-176">Nepoužívejte `deleted` jako vlastnost v třídě.</span><span class="sxs-lookup"><span data-stu-id="e3624-176">Do not use `deleted` as a property in your class.</span></span>

<span data-ttu-id="e3624-177">Hello `id` pole je povinné.</span><span class="sxs-lookup"><span data-stu-id="e3624-177">hello `id` field is required.</span></span>  <span data-ttu-id="e3624-178">Hello `updatedAt` pole a `version` pole se používají pro offline synchronizace (pro přírůstkové synchronizace a dojde ke konfliktu řešení v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="e3624-178">hello `updatedAt` field and `version` field are used for offline synchronization (for incremental sync and conflict resolution respectively).</span></span>  <span data-ttu-id="e3624-179">Hello `createdAt` pole je odkaz na pole a není používán hello klienta.</span><span class="sxs-lookup"><span data-stu-id="e3624-179">hello `createdAt` field is a reference field and is not used by hello client.</span></span>  <span data-ttu-id="e3624-180">Hello názvy jsou názvy "přes přenosu" hello vlastností a nejsou upravit.</span><span class="sxs-lookup"><span data-stu-id="e3624-180">hello names are "across-the-wire" names of hello properties and are not adjustable.</span></span>  <span data-ttu-id="e3624-181">Však vytvořit mapování mezi objektem a hello "přes přenosu" názvy pomocí hello [gson] [ 3] knihovny.</span><span class="sxs-lookup"><span data-stu-id="e3624-181">However, you can create a mapping between your object and hello "across-the-wire" names using hello [gson][3] library.</span></span>  <span data-ttu-id="e3624-182">Například:</span><span class="sxs-lookup"><span data-stu-id="e3624-182">For example:</span></span>

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getText() { return mVersion; }
    public final void setText(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a><span data-ttu-id="e3624-183">Vytvořit odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="e3624-183">Create a Table Reference</span></span>

<span data-ttu-id="e3624-184">tooaccess tabulku, nejprve vytvořit [MobileServiceTable] [ 8] objekt ve volání hello **jít** metodu hello [MobileServiceClient][9].</span><span class="sxs-lookup"><span data-stu-id="e3624-184">tooaccess a table, first create a [MobileServiceTable][8] object by calling hello **getTable** method on hello [MobileServiceClient][9].</span></span>  <span data-ttu-id="e3624-185">Tato metoda má dva přetížení:</span><span class="sxs-lookup"><span data-stu-id="e3624-185">This method has two overloads:</span></span>

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

<span data-ttu-id="e3624-186">V následující kód, hello **mClient** je MobileServiceClient tooyour referenční objekt.</span><span class="sxs-lookup"><span data-stu-id="e3624-186">In hello following code, **mClient** is a reference tooyour MobileServiceClient object.</span></span>  <span data-ttu-id="e3624-187">první přetížení Hello se používá, kde název třídy hello a název tabulky hello jsou hello stejné, a hello jeden slouží v hello rychlý start:</span><span class="sxs-lookup"><span data-stu-id="e3624-187">hello first overload is used where hello class name and hello table name are hello same, and is hello one used in hello Quickstart:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

<span data-ttu-id="e3624-188">Hello druhý přetížení se používá při hello název tabulky se liší od názvu třídy hello: první parametr hello je název tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-188">hello second overload is used when hello table name is different from hello class name: hello first parameter is hello table name.</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <span data-ttu-id="e3624-189"><a name="query"></a>Dotazování tabulky back-end</span><span class="sxs-lookup"><span data-stu-id="e3624-189"><a name="query"></a>Query a Backend Table</span></span>

<span data-ttu-id="e3624-190">Nejprve získejte odkaz na tabulku.</span><span class="sxs-lookup"><span data-stu-id="e3624-190">First, obtain a table reference.</span></span>  <span data-ttu-id="e3624-191">Potom spusťte dotaz na odkaz na tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-191">Then execute a query on hello table reference.</span></span>  <span data-ttu-id="e3624-192">Dotaz je libovolnou kombinaci:</span><span class="sxs-lookup"><span data-stu-id="e3624-192">A query is any combination of:</span></span>

* <span data-ttu-id="e3624-193">A `.where()` [klauzuli filtru](#filtering).</span><span class="sxs-lookup"><span data-stu-id="e3624-193">A `.where()` [filter clause](#filtering).</span></span>
* <span data-ttu-id="e3624-194">`.orderBy()` [Řazení klauzule](#sorting).</span><span class="sxs-lookup"><span data-stu-id="e3624-194">An `.orderBy()` [ordering clause](#sorting).</span></span>
* <span data-ttu-id="e3624-195">A `.select()` [klauzule výběr pole](#selection).</span><span class="sxs-lookup"><span data-stu-id="e3624-195">A `.select()` [field selection clause](#selection).</span></span>
* <span data-ttu-id="e3624-196">A `.skip()` a `.top()` pro [stránkovaného výsledky](#paging).</span><span class="sxs-lookup"><span data-stu-id="e3624-196">A `.skip()` and `.top()` for [paged results](#paging).</span></span>

<span data-ttu-id="e3624-197">klauzule Hello musí uvedené v předcházející pořadí hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-197">hello clauses must be presented in hello preceding order.</span></span>

### <span data-ttu-id="e3624-198"><a name="filter"></a>Filtrování výsledků</span><span class="sxs-lookup"><span data-stu-id="e3624-198"><a name="filter"></a> Filtering Results</span></span>

<span data-ttu-id="e3624-199">Obecné formuláře Hello dotazu je:</span><span class="sxs-lookup"><span data-stu-id="e3624-199">hello general form of a query is:</span></span>

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

<span data-ttu-id="e3624-200">Hello předchozí příklad vrací všechny výsledky (až toohello maximální velikost stránky nastavená serverem hello).</span><span class="sxs-lookup"><span data-stu-id="e3624-200">hello preceding example returns all results (up toohello maximum page size set by hello server).</span></span>  <span data-ttu-id="e3624-201">Hello `.execute()` metoda provede hello dotaz na back-end hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-201">hello `.execute()` method executes hello query on hello backend.</span></span>  <span data-ttu-id="e3624-202">Hello dotaz je převeden tooan [OData v3] [ 19] Dotázat se před přenosem toohello Mobile Apps back-end.</span><span class="sxs-lookup"><span data-stu-id="e3624-202">hello query is converted tooan [OData v3][19] query before transmission toohello Mobile Apps backend.</span></span>  <span data-ttu-id="e3624-203">Back-end mobilní aplikace hello na přijetí, převede hello dotazu příkazu SQL před provedením na instanci SQL Azure hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-203">On receipt, hello Mobile Apps backend converts hello query into an SQL statement before executing it on hello SQL Azure instance.</span></span>  <span data-ttu-id="e3624-204">Vzhledem k tomu, že nějakou dobu trvá síťové aktivity, hello `.execute()` metoda vrátí [ `ListenableFuture<E>` ] [ 18].</span><span class="sxs-lookup"><span data-stu-id="e3624-204">Since network activity takes some time, hello `.execute()` method returns a [`ListenableFuture<E>`][18].</span></span>

### <span data-ttu-id="e3624-205"><a name="filtering"></a>Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="e3624-205"><a name="filtering"></a>Filter returned data</span></span>

<span data-ttu-id="e3624-206">Hello následující spuštění dotazu vrátí všechny položky z hello **ToDoItem** tabulky kde **dokončení** rovná **false**.</span><span class="sxs-lookup"><span data-stu-id="e3624-206">hello following query execution returns all items from hello **ToDoItem** table where **complete** equals **false**.</span></span>

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

<span data-ttu-id="e3624-207">**mToDoTable** je hello referenční toohello mobilní služby tabulku, kterou jsme vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e3624-207">**mToDoTable** is hello reference toohello mobile service table that we created previously.</span></span>

<span data-ttu-id="e3624-208">Definujte filtr pomocí hello **kde** volání metody na odkaz na tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-208">Define a filter using hello **where** method call on hello table reference.</span></span> <span data-ttu-id="e3624-209">Hello **kde** metoda následuje **pole** metoda následuje metodu, která určuje hello logické predikátu.</span><span class="sxs-lookup"><span data-stu-id="e3624-209">hello **where** method is followed by a **field** method followed by a method that specifies hello logical predicate.</span></span> <span data-ttu-id="e3624-210">Zahrnout možných metod predikátem **eq** (rovná), **ne** (nerovná), **gt** (větší než), **ge** (větší než nebo rovno), **lt** (méně než), **le** (je menší než nebo rovno).</span><span class="sxs-lookup"><span data-stu-id="e3624-210">Possible predicate methods include **eq** (equals), **ne** (not equal), **gt** (greater than), **ge** (greater than or equal to), **lt** (less than), **le** (less than or equal to).</span></span> <span data-ttu-id="e3624-211">Tyto metody umožňují porovnat číslo a řetězec polí toospecific hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e3624-211">These methods let you compare number and string fields toospecific values.</span></span>

<span data-ttu-id="e3624-212">Můžete filtrovat podle data.</span><span class="sxs-lookup"><span data-stu-id="e3624-212">You can filter on dates.</span></span> <span data-ttu-id="e3624-213">Hello následující metody umožňují porovnat pole pro celý datum hello nebo její části datum hello: **roku**, **měsíc**, **den**, **hodinu**, **minutu**, a **druhý**.</span><span class="sxs-lookup"><span data-stu-id="e3624-213">hello following methods let you compare hello entire date field or parts of hello date: **year**, **month**, **day**, **hour**, **minute**, and **second**.</span></span> <span data-ttu-id="e3624-214">Hello následující příklad přidá filtr pro položky jejichž *datum splatnosti* rovná 2013.</span><span class="sxs-lookup"><span data-stu-id="e3624-214">hello following example adds a filter for items whose *due date* equals 2013.</span></span>

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

<span data-ttu-id="e3624-215">Hello následující metody podporují komplexní filtry na polí s řetězcem: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **nahradit**, **toLower**, **toUpper**, **trim**, a  **Délka**.</span><span class="sxs-lookup"><span data-stu-id="e3624-215">hello following methods support complex filters on string fields: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, and **length**.</span></span> <span data-ttu-id="e3624-216">Následující příklad filtry pro tabulku řádků, kde hello Hello *text* sloupec začíná "PRI0."</span><span class="sxs-lookup"><span data-stu-id="e3624-216">hello following example filters for table rows where hello *text* column starts with "PRI0."</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="e3624-217">podporovány jsou Hello následující metody operátor pro pole s počtem: **přidat**, **sub**, **mul**, **div**, **mod**, **podlaží**, **mezní hodnoty**, a **ZAOKROUHLIT**.</span><span class="sxs-lookup"><span data-stu-id="e3624-217">hello following operator methods are supported on number fields: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**, and **round**.</span></span> <span data-ttu-id="e3624-218">Následující příklad filtry pro tabulku řádků, kde hello Hello **doba trvání** je číslo sudé.</span><span class="sxs-lookup"><span data-stu-id="e3624-218">hello following example filters for table rows where hello **duration** is an even number.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

<span data-ttu-id="e3624-219">Predikáty můžete kombinovat s tyto logické metody: **a**, **nebo** a **není**.</span><span class="sxs-lookup"><span data-stu-id="e3624-219">You can combine predicates with these logical methods: **and**, **or** and **not**.</span></span> <span data-ttu-id="e3624-220">Následující ukázka Hello kombinuje dvě hello předcházející příklady.</span><span class="sxs-lookup"><span data-stu-id="e3624-220">hello following example combines two of hello preceding examples.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="e3624-221">Skupiny a vnořit logické operátory:</span><span class="sxs-lookup"><span data-stu-id="e3624-221">Group and nest logical operators:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

<span data-ttu-id="e3624-222">Podrobnější diskuzi a příklady filtrování, najdete v části [zkoumat hello bohatost modelu dotazu Android klienta hello][20].</span><span class="sxs-lookup"><span data-stu-id="e3624-222">For more detailed discussion and examples of filtering, see [Exploring hello richness of hello Android client query model][20].</span></span>

### <span data-ttu-id="e3624-223"><a name="sorting"></a>Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="e3624-223"><a name="sorting"></a>Sort returned data</span></span>

<span data-ttu-id="e3624-224">Hello následující kód vrátí všechny položky z tabulky **ToDoItems** seřadit vzestupně podle hello *text* pole.</span><span class="sxs-lookup"><span data-stu-id="e3624-224">hello following code returns all items from a table of **ToDoItems** sorted ascending by hello *text* field.</span></span> <span data-ttu-id="e3624-225">*mToDoTable* je hello referenční toohello back-end tabulku, kterou jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="e3624-225">*mToDoTable* is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

<span data-ttu-id="e3624-226">první parametr hello Hello **orderBy** metoda je rovna toohello názvu řetězce hello pole, na které toosort.</span><span class="sxs-lookup"><span data-stu-id="e3624-226">hello first parameter of hello **orderBy** method is a string equal toohello name of hello field on which toosort.</span></span> <span data-ttu-id="e3624-227">druhý parametr Hello používá hello **QueryOrder** výčtu toospecify zda toosort vzestupném nebo sestupném.</span><span class="sxs-lookup"><span data-stu-id="e3624-227">hello second parameter uses hello **QueryOrder** enumeration toospecify whether toosort ascending or descending.</span></span>  <span data-ttu-id="e3624-228">Filtrování pomocí hello ***kde*** metoda, hello ***kde*** metoda musí být volána před hello ***orderBy*** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-228">If you are filtering using hello ***where*** method, hello ***where*** method must be invoked before hello ***orderBy*** method.</span></span>

### <span data-ttu-id="e3624-229"><a name="selection"></a>Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="e3624-229"><a name="selection"></a>Select specific columns</span></span>

<span data-ttu-id="e3624-230">Hello následující kód ukazuje, jak tooreturn všechny položky z tabulky **ToDoItems**, ale zobrazuje jenom hello **dokončení** a **text** pole.</span><span class="sxs-lookup"><span data-stu-id="e3624-230">hello following code illustrates how tooreturn all items from a table of **ToDoItems**, but only displays hello **complete** and **text** fields.</span></span> <span data-ttu-id="e3624-231">**mToDoTable** je hello referenční toohello back-end tabulku, kterou jsme vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e3624-231">**mToDoTable** is hello reference toohello backend table that we created previously.</span></span>

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

<span data-ttu-id="e3624-232">Vyberte funkce toohello Hello parametry jsou hello řetězec názvy sloupců hello tabulky, které chcete tooreturn.</span><span class="sxs-lookup"><span data-stu-id="e3624-232">hello parameters toohello select function are hello string names of hello table's columns that you want tooreturn.</span></span>  <span data-ttu-id="e3624-233">Hello **vyberte** toofollow metody, třeba musí metoda **kde** a **orderBy**.</span><span class="sxs-lookup"><span data-stu-id="e3624-233">hello **select** method needs toofollow methods like **where** and **orderBy**.</span></span> <span data-ttu-id="e3624-234">Může následovat stránkování metody, třeba **přeskočit** a **horní**.</span><span class="sxs-lookup"><span data-stu-id="e3624-234">It can be followed by paging methods like **skip** and **top**.</span></span>

### <span data-ttu-id="e3624-235"><a name="paging"></a>Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="e3624-235"><a name="paging"></a>Return data in pages</span></span>

<span data-ttu-id="e3624-236">Data jsou **vždy** vrátil na stránkách.</span><span class="sxs-lookup"><span data-stu-id="e3624-236">Data is **ALWAYS** returned in pages.</span></span>  <span data-ttu-id="e3624-237">maximální počet záznamů vrácených Hello nastavena serverem hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-237">hello maximum number of records returned is set by hello server.</span></span>  <span data-ttu-id="e3624-238">Pokud hello klient požádá o další záznamy, vrátí hello server hello maximální počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="e3624-238">If hello client requests more records, then hello server returns hello maximum number of records.</span></span>  <span data-ttu-id="e3624-239">Ve výchozím nastavení je hello maximální velikost stránky na hello server 50 záznamů.</span><span class="sxs-lookup"><span data-stu-id="e3624-239">By default, hello maximum page size on hello server is 50 records.</span></span>

<span data-ttu-id="e3624-240">Hello první příklad ukazuje, jak tooselect hello prvních pět položek z tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3624-240">hello first example shows how tooselect hello top five items from a table.</span></span> <span data-ttu-id="e3624-241">Hello dotaz vrátí hello položky z tabulky **ToDoItems**.</span><span class="sxs-lookup"><span data-stu-id="e3624-241">hello query returns hello items from a table of **ToDoItems**.</span></span> <span data-ttu-id="e3624-242">**mToDoTable** je hello referenční toohello back-end tabulku, kterou jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="e3624-242">**mToDoTable** is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

<span data-ttu-id="e3624-243">Zde je dotaz přeskočí hello prvních pět položek, a potom vrátí hello další pět:</span><span class="sxs-lookup"><span data-stu-id="e3624-243">Here's a query that skips hello first five items, and then returns hello next five:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

<span data-ttu-id="e3624-244">Pokud chcete tooget všechny záznamy v tabulce implementaci kódu tooiterate přes všechny stránky:</span><span class="sxs-lookup"><span data-stu-id="e3624-244">If you wish tooget all records in a table, implement code tooiterate over all pages:</span></span>

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

<span data-ttu-id="e3624-245">Žádost o pro všechny záznamy pomocí této metody vytvoří minimálně dva požadavky toohello Mobile Apps back-end.</span><span class="sxs-lookup"><span data-stu-id="e3624-245">A request for all records using this method creates a minimum of two requests toohello Mobile Apps backend.</span></span>

> [!TIP]
> <span data-ttu-id="e3624-246">Výběrem možnosti velikost pravá stránka hello je rovnováhu mezi využití paměti při žádosti o hello se děje, využití šířky pásma a zpoždění při příjmu dat hello úplně.</span><span class="sxs-lookup"><span data-stu-id="e3624-246">Choosing hello right page size is a balance between memory usage while hello request is happening, bandwidth usage and delay in receiving hello data completely.</span></span>  <span data-ttu-id="e3624-247">Výchozí Hello (50 záznamy) je vhodná pro všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="e3624-247">hello default (50 records) is suitable for all devices.</span></span>  <span data-ttu-id="e3624-248">Pokud provozujete výhradně na větší paměti zařízení, zvýšit až too500.</span><span class="sxs-lookup"><span data-stu-id="e3624-248">If you exclusively operate on larger memory devices, increase up too500.</span></span>  <span data-ttu-id="e3624-249">Našli jsme tato roste velikost stránky hello nad rámec 500 zaznamenává výsledky v zpožděním a velké paměti problémy.</span><span class="sxs-lookup"><span data-stu-id="e3624-249">We have found that increasing hello page size beyond 500 records results in unacceptable delays and large memory issues.</span></span>

### <span data-ttu-id="e3624-250"><a name="chaining"></a>Postupy: řetězení metody dotazů</span><span class="sxs-lookup"><span data-stu-id="e3624-250"><a name="chaining"></a>How to: Concatenate query methods</span></span>

<span data-ttu-id="e3624-251">může být zřetězen Hello metody používané v dotazování tabulky back-end.</span><span class="sxs-lookup"><span data-stu-id="e3624-251">hello methods used in querying backend tables can be concatenated.</span></span> <span data-ttu-id="e3624-252">Řetězení dotazu metody vám umožní tooselect určité sloupce filtrované řádků, které jsou seřazené a stránkovaného fondu.</span><span class="sxs-lookup"><span data-stu-id="e3624-252">Chaining query methods allows you tooselect specific columns of filtered rows that are sorted and paged.</span></span> <span data-ttu-id="e3624-253">Můžete vytvořit komplexní logické filtry.</span><span class="sxs-lookup"><span data-stu-id="e3624-253">You can create complex logical filters.</span></span>  <span data-ttu-id="e3624-254">Každá metoda dotaz vrátí objekt dotazu.</span><span class="sxs-lookup"><span data-stu-id="e3624-254">Each query method returns a Query object.</span></span> <span data-ttu-id="e3624-255">tooend hello řady metod a ve skutečnosti spuštění hello dotaz, volání hello **provést** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-255">tooend hello series of methods and actually run hello query, call hello **execute** method.</span></span> <span data-ttu-id="e3624-256">Například:</span><span class="sxs-lookup"><span data-stu-id="e3624-256">For example:</span></span>

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

<span data-ttu-id="e3624-257">Hello zřetězené dotaz, který metody musejí být seřazeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e3624-257">hello chained query methods must be ordered as follows:</span></span>

1. <span data-ttu-id="e3624-258">Filtrování (**kde**) metody.</span><span class="sxs-lookup"><span data-stu-id="e3624-258">Filtering (**where**) methods.</span></span>
2. <span data-ttu-id="e3624-259">Řazení (**orderBy**) metody.</span><span class="sxs-lookup"><span data-stu-id="e3624-259">Sorting (**orderBy**) methods.</span></span>
3. <span data-ttu-id="e3624-260">Výběr (**vyberte**) metody.</span><span class="sxs-lookup"><span data-stu-id="e3624-260">Selection (**select**) methods.</span></span>
4. <span data-ttu-id="e3624-261">stránkování (**přeskočit** a **horní**) metody.</span><span class="sxs-lookup"><span data-stu-id="e3624-261">paging (**skip** and **top**) methods.</span></span>

## <span data-ttu-id="e3624-262"><a name="binding"></a>Vytvoření vazby dat toohello uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="e3624-262"><a name="binding"></a>Bind data toohello user interface</span></span>

<span data-ttu-id="e3624-263">Datová vazba zahrnuje tři komponenty:</span><span class="sxs-lookup"><span data-stu-id="e3624-263">Data binding involves three components:</span></span>

* <span data-ttu-id="e3624-264">zdroj dat Hello</span><span class="sxs-lookup"><span data-stu-id="e3624-264">hello data source</span></span>
* <span data-ttu-id="e3624-265">rozložení obrazovky Hello</span><span class="sxs-lookup"><span data-stu-id="e3624-265">hello screen layout</span></span>
* <span data-ttu-id="e3624-266">adaptér Hello že ties hello dva společně.</span><span class="sxs-lookup"><span data-stu-id="e3624-266">hello adapter that ties hello two together.</span></span>

<span data-ttu-id="e3624-267">V našem ukázkový kód vrátíme hello data z tabulky Mobile Apps SQL Azure hello **ToDoItem** na pole.</span><span class="sxs-lookup"><span data-stu-id="e3624-267">In our sample code, we return hello data from hello Mobile Apps SQL Azure table **ToDoItem** into an array.</span></span> <span data-ttu-id="e3624-268">Tato aktivita je běžný vzor pro data aplikací.</span><span class="sxs-lookup"><span data-stu-id="e3624-268">This activity is a common pattern for data applications.</span></span>  <span data-ttu-id="e3624-269">Databázové dotazy často vracet kolekci řádků, které hello klient získá v seznamu nebo pole.</span><span class="sxs-lookup"><span data-stu-id="e3624-269">Database queries often return a collection of rows that hello client gets in a list or array.</span></span> <span data-ttu-id="e3624-270">V této ukázce hello pole je zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-270">In this sample, hello array is hello data source.</span></span>  <span data-ttu-id="e3624-271">Kód Hello určuje rozložení obrazovky, který definuje zobrazení hello hello dat, který se zobrazí na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-271">hello code specifies a screen layout that defines hello view of hello data that appears on hello device.</span></span>  <span data-ttu-id="e3624-272">Hello dva jsou svázané s společně s adaptér, který tento kód je rozšířením hello **ArrayAdapter&lt;ToDoItem&gt;**  třídy.</span><span class="sxs-lookup"><span data-stu-id="e3624-272">hello two are bound together with an adapter, which in this code is an extension of hello **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span>

#### <span data-ttu-id="e3624-273"><a name="layout"></a>Definování hello rozložení</span><span class="sxs-lookup"><span data-stu-id="e3624-273"><a name="layout"></a>Define hello Layout</span></span>

<span data-ttu-id="e3624-274">rozložení Hello je definována několik fragmenty kódu XML.</span><span class="sxs-lookup"><span data-stu-id="e3624-274">hello layout is defined by several snippets of XML code.</span></span> <span data-ttu-id="e3624-275">Existující rozložení zadána hello následující kód představuje hello **ListView** chceme toopopulate pomocí našich dat serveru.</span><span class="sxs-lookup"><span data-stu-id="e3624-275">Given an existing layout, hello following code represents hello **ListView** we want toopopulate with our server data.</span></span>

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

<span data-ttu-id="e3624-276">V předchozích kód hello, hello *listitem* atribut určuje hello id hello rozložení jednotlivých řádků v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-276">In hello preceding code, hello *listitem* attribute specifies hello id of hello layout for an individual row in hello list.</span></span> <span data-ttu-id="e3624-277">Tento kód určuje zaškrtávací políčko a jeho přidružené textu a získá instanci jednou pro každou položku v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-277">This code specifies a check box and its associated text and gets instantiated once for each item in hello list.</span></span> <span data-ttu-id="e3624-278">Toto rozložení nezobrazí hello **id** pole a složitější rozložení by zadejte další pole v zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-278">This layout does not display hello **id** field, and a more complex layout would specify additional fields in hello display.</span></span> <span data-ttu-id="e3624-279">Tento kód je v hello **row_list_to_do.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="e3624-279">This code is in hello **row_list_to_do.xml** file.</span></span>

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <span data-ttu-id="e3624-280"><a name="adapter"></a>Zadejte adaptér hello</span><span class="sxs-lookup"><span data-stu-id="e3624-280"><a name="adapter"></a>Define hello adapter</span></span>
<span data-ttu-id="e3624-281">Vzhledem k tomu, že zdroj dat hello naše zobrazení je pole **ToDoItem**, jsme podtřídami adaptéru v našem **ArrayAdapter&lt;ToDoItem&gt;**  třídy.</span><span class="sxs-lookup"><span data-stu-id="e3624-281">Since hello data source of our view is an array of **ToDoItem**, we subclass our adapter from an **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span> <span data-ttu-id="e3624-282">Tato podtřídami vytvoří zobrazení pro každý **ToDoItem** pomocí hello **row_list_to_do** rozložení.</span><span class="sxs-lookup"><span data-stu-id="e3624-282">This subclass produces a View for every **ToDoItem** using hello **row_list_to_do** layout.</span></span>  <span data-ttu-id="e3624-283">V našem kódu jsme definovali hello následující třídy, která je rozšířením hello **ArrayAdapter&lt;E&gt;**  třídy:</span><span class="sxs-lookup"><span data-stu-id="e3624-283">In our code, we define hello following class that is an extension of hello **ArrayAdapter&lt;E&gt;** class:</span></span>

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

<span data-ttu-id="e3624-284">Přepsání hello adaptéry **getView** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-284">Override hello adapters **getView** method.</span></span> <span data-ttu-id="e3624-285">Například:</span><span class="sxs-lookup"><span data-stu-id="e3624-285">For example:</span></span>

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

<span data-ttu-id="e3624-286">Vytvoříme instance této třídy v našem aktivity následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e3624-286">We create an instance of this class in our Activity as follows:</span></span>

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

<span data-ttu-id="e3624-287">Hello druhý parametr toohello ToDoItemAdapter konstruktor je rozložení toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="e3624-287">hello second parameter toohello ToDoItemAdapter constructor is a reference toohello layout.</span></span> <span data-ttu-id="e3624-288">Jsme teď vytvořit instanci hello **ListView** a přiřaďte hello adaptér toohello **ListView**.</span><span class="sxs-lookup"><span data-stu-id="e3624-288">We can now instantiate hello **ListView** and assign hello adapter toohello **ListView**.</span></span>

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <span data-ttu-id="e3624-289"><a name="use-adapter"></a>Použít hello adaptér tooBind toohello uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="e3624-289"><a name="use-adapter"></a>Use hello Adapter tooBind toohello UI</span></span>

<span data-ttu-id="e3624-290">Nyní je připraven toouse datové vazby.</span><span class="sxs-lookup"><span data-stu-id="e3624-290">You are now ready toouse data binding.</span></span> <span data-ttu-id="e3624-291">Hello následující kód ukazuje, jak tooget položky v tabulce hello a výplněmi hello místní adaptéru s hello vrátil položky.</span><span class="sxs-lookup"><span data-stu-id="e3624-291">hello following code shows how tooget items in hello table and fills hello local adapter with hello returned items.</span></span>

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

<span data-ttu-id="e3624-292">Volání hello adaptér kdykoli upravit hello **ToDoItem** tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3624-292">Call hello adapter any time you modify hello **ToDoItem** table.</span></span> <span data-ttu-id="e3624-293">Vzhledem k tomu, že změny se provádí na základě záznamu podle, můžete zpracovat jeden řádek místo kolekce.</span><span class="sxs-lookup"><span data-stu-id="e3624-293">Since modifications are done on a record by record basis, you handle a single row instead of a collection.</span></span> <span data-ttu-id="e3624-294">Při vložení položku volání hello **přidat** metoda na hello adaptér; při odstraňování, volání hello **odebrat** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-294">When you insert an item, call hello **add** method on hello adapter; when deleting, call hello **remove** method.</span></span>

<span data-ttu-id="e3624-295">Úplný příklad můžete najít v hello [projekt Android rychlý Start][21].</span><span class="sxs-lookup"><span data-stu-id="e3624-295">You can find a complete example in hello [Android Quickstart Project][21].</span></span>

## <span data-ttu-id="e3624-296"><a name="inserting"></a>Vložení dat do back-end hello</span><span class="sxs-lookup"><span data-stu-id="e3624-296"><a name="inserting"></a>Insert data into hello backend</span></span>

<span data-ttu-id="e3624-297">Vytvoří instanci hello *ToDoItem* a nastavit jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e3624-297">Instantiate an instance of hello *ToDoItem* class and set its properties.</span></span>

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

<span data-ttu-id="e3624-298">Potom pomocí **insert()** tooinsert objekt:</span><span class="sxs-lookup"><span data-stu-id="e3624-298">Then use **insert()** tooinsert an object:</span></span>

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="e3624-299">Hello vrátil entity odpovídá hello data vložená do tabulky hello back-end, zahrnuté hello ID a všechny ostatní hodnoty (například hello `createdAt`, `updatedAt`, a `version` polí) nastavte na back-end hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-299">hello returned entity matches hello data inserted into hello backend table, included hello ID and any other values (such as hello `createdAt`, `updatedAt`, and `version` fields) set on hello backend.</span></span>

<span data-ttu-id="e3624-300">Mobile Apps tabulky vyžadují sloupec primárního klíče s názvem **id**. Tento sloupec musí být řetězec.</span><span class="sxs-lookup"><span data-stu-id="e3624-300">Mobile Apps tables require a primary key column named **id**. This column must be a string.</span></span> <span data-ttu-id="e3624-301">Výchozí hodnota Hello hello ID sloupce je identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="e3624-301">hello default value of hello ID column is a GUID.</span></span>  <span data-ttu-id="e3624-302">Můžete zadat další jedinečné hodnoty, například e-mailové adresy nebo uživatelských jmen.</span><span class="sxs-lookup"><span data-stu-id="e3624-302">You can provide other unique values, such as email addresses or usernames.</span></span> <span data-ttu-id="e3624-303">Pokud není zadána hodnota ID řetězec vloženého záznamu, back-end hello generuje nový identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="e3624-303">When a string ID value is not provided for an inserted record, hello backend generates a new GUID.</span></span>

<span data-ttu-id="e3624-304">Řetězcové hodnoty ID poskytují hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e3624-304">String ID values provide hello following advantages:</span></span>

* <span data-ttu-id="e3624-305">ID může být generována bez provedení databáze toohello odezvy.</span><span class="sxs-lookup"><span data-stu-id="e3624-305">IDs can be generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="e3624-306">Záznamy jsou jednodušší toomerge z různých tabulek nebo databází.</span><span class="sxs-lookup"><span data-stu-id="e3624-306">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="e3624-307">Hodnoty ID lépe integrovat logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-307">ID values integrate better with an application's logic.</span></span>

<span data-ttu-id="e3624-308">Řetězec ID hodnoty jsou **REQUIRED** pro podporu offline synchronizace.</span><span class="sxs-lookup"><span data-stu-id="e3624-308">String ID values are **REQUIRED** for offline sync support.</span></span>  <span data-ttu-id="e3624-309">Id nelze změnit, jakmile je uložen v databázi back-end hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-309">You cannot change an Id once it is stored in hello backend database.</span></span>

## <span data-ttu-id="e3624-310"><a name="updating"></a>Aktualizovat data v mobilní aplikaci</span><span class="sxs-lookup"><span data-stu-id="e3624-310"><a name="updating"></a>Update data in a mobile app</span></span>

<span data-ttu-id="e3624-311">tooupdate data v tabulce, předat hello nový objekt toohello **update()** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-311">tooupdate data in a table, pass hello new object toohello **update()** method.</span></span>

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="e3624-312">V tomto příkladu *položky* je odkaz na řádek tooa v hello *ToDoItem* tabulku, která se použila tooit některé změny provedené v.</span><span class="sxs-lookup"><span data-stu-id="e3624-312">In this example, *item* is a reference tooa row in hello *ToDoItem* table, which has had some changes made tooit.</span></span>  <span data-ttu-id="e3624-313">Hello řádek s hello stejné **id** se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="e3624-313">hello row with hello same **id** is updated.</span></span>

## <span data-ttu-id="e3624-314"><a name="deleting"></a>Odstranit data v mobilní aplikaci</span><span class="sxs-lookup"><span data-stu-id="e3624-314"><a name="deleting"></a>Delete data in a mobile app</span></span>

<span data-ttu-id="e3624-315">Hello následující kód ukazuje, jak toodelete data z tabulky zadáním hello datový objekt.</span><span class="sxs-lookup"><span data-stu-id="e3624-315">hello following code shows how toodelete data from a table by specifying hello data object.</span></span>

```java
mToDoTable
    .delete(item);
```

<span data-ttu-id="e3624-316">Můžete také odstranit položku zadáním hello **id** pole z toodelete řádek hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-316">You can also delete an item by specifying hello **id** field of hello row toodelete.</span></span>

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <span data-ttu-id="e3624-317"><a name="lookup"></a>Vyhledat konkrétní položky podle Id</span><span class="sxs-lookup"><span data-stu-id="e3624-317"><a name="lookup"></a>Look up a specific item by Id</span></span>

<span data-ttu-id="e3624-318">Vyhledání položky s konkrétní **id** pole s hello **lookUp()** metoda:</span><span class="sxs-lookup"><span data-stu-id="e3624-318">Look up an item with a specific **id** field with hello **lookUp()** method:</span></span>

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <span data-ttu-id="e3624-319"><a name="untyped"></a>Postupy: práce s daty bez typu</span><span class="sxs-lookup"><span data-stu-id="e3624-319"><a name="untyped"></a>How to: Work with untyped data</span></span>

<span data-ttu-id="e3624-320">netypové programovací model Hello vám dává přesnou kontrolu nad serializace JSON.</span><span class="sxs-lookup"><span data-stu-id="e3624-320">hello untyped programming model gives you exact control over JSON serialization.</span></span>  <span data-ttu-id="e3624-321">Existují některé běžné scénáře, kde můžete toouse netypové programovací model.</span><span class="sxs-lookup"><span data-stu-id="e3624-321">There are some common scenarios where you may wish toouse an untyped programming model.</span></span> <span data-ttu-id="e3624-322">Například pokud back-end tabulka obsahuje mnoho sloupců a potřebujete jenom tooreference podmnožinu sloupců hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-322">For example, if your backend table contains many columns and you only need tooreference a subset of hello columns.</span></span>  <span data-ttu-id="e3624-323">Hello typu modelu vyžaduje toodefine všechny hello sloupce definované v back-end mobilní aplikace hello v třídě data.</span><span class="sxs-lookup"><span data-stu-id="e3624-323">hello typed model requires you toodefine all hello columns defined in hello Mobile Apps backend in your data class.</span></span>  <span data-ttu-id="e3624-324">Většina hello volání rozhraní API pro přístup k datům jsou podobné toohello zadali programovací volání.</span><span class="sxs-lookup"><span data-stu-id="e3624-324">Most of hello API calls for accessing data are similar toohello typed programming calls.</span></span> <span data-ttu-id="e3624-325">Hello hlavní rozdíl je, že v modelu netypové hello volat metody na hello **MobileServiceJsonTable** objektu, nikoli hello **MobileServiceTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="e3624-325">hello main difference is that in hello untyped model you invoke methods on hello **MobileServiceJsonTable** object, instead of hello **MobileServiceTable** object.</span></span>

### <span data-ttu-id="e3624-326"><a name="json_instance"></a>Vytvoření instance bez typu tabulky</span><span class="sxs-lookup"><span data-stu-id="e3624-326"><a name="json_instance"></a>Create an instance of an untyped table</span></span>

<span data-ttu-id="e3624-327">Podobné toohello typu modelu, můžete začít nastavením odkaz na tabulku, ale v takovém případě je **MobileServicesJsonTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="e3624-327">Similar toohello typed model, you start by getting a table reference, but in this case it's a **MobileServicesJsonTable** object.</span></span> <span data-ttu-id="e3624-328">Získejte odkaz na hello volání hello **jít** metoda na instanci hello klienta:</span><span class="sxs-lookup"><span data-stu-id="e3624-328">Obtain hello reference by calling hello **getTable** method on an instance of hello client:</span></span>

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

<span data-ttu-id="e3624-329">Po vytvoření instance hello **MobileServiceJsonTable**, ho má prakticky hello stejné rozhraní API k dispozici jako s typem programovací model hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-329">Once you have created an instance of hello **MobileServiceJsonTable**, it has virtually hello same API available as with hello typed programming model.</span></span> <span data-ttu-id="e3624-330">V některých případech hello metody přijímají bez typu parametru místo typu parametru.</span><span class="sxs-lookup"><span data-stu-id="e3624-330">In some cases, hello methods take an untyped parameter instead of a typed parameter.</span></span>

### <span data-ttu-id="e3624-331"><a name="json_insert"></a>Vložit do tabulky bez typu</span><span class="sxs-lookup"><span data-stu-id="e3624-331"><a name="json_insert"></a>Insert into an untyped table</span></span>
<span data-ttu-id="e3624-332">Následující kód ukazuje, jak Hello toodo typu vložení.</span><span class="sxs-lookup"><span data-stu-id="e3624-332">hello following code shows how toodo an insert.</span></span> <span data-ttu-id="e3624-333">Hello prvním krokem je toocreate [JsonObject][1], který je součástí hello [gson] [ 3] knihovny.</span><span class="sxs-lookup"><span data-stu-id="e3624-333">hello first step is toocreate a [JsonObject][1], which is part of hello [gson][3] library.</span></span>

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

<span data-ttu-id="e3624-334">Poté použijte **insert()** tooinsert hello bez typu objektu do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-334">Then, Use **insert()** tooinsert hello untyped object into hello table.</span></span>

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

<span data-ttu-id="e3624-335">Pokud potřebujete tooget hello ID objektu hello vložit, použijte hello **getAsJsonPrimitive()** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-335">If you need tooget hello ID of hello inserted object, use hello **getAsJsonPrimitive()** method.</span></span>

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <span data-ttu-id="e3624-336"><a name="json_delete"></a>Odstraňte z bez typu tabulky</span><span class="sxs-lookup"><span data-stu-id="e3624-336"><a name="json_delete"></a>Delete from an untyped table</span></span>
<span data-ttu-id="e3624-337">Hello následující kód ukazuje, jak toodelete na instance, v takovém případě hello stejnou instanci **JsonObject** který byl vytvořen v hello před *vložit* příklad.</span><span class="sxs-lookup"><span data-stu-id="e3624-337">hello following code shows how toodelete an instance, in this case, hello same instance of a **JsonObject** that was created in hello prior *insert* example.</span></span> <span data-ttu-id="e3624-338">Kód Hello je hello stejné jako u hello zadali případ, ale metoda hello má jiný podpis, protože odkazuje na **JsonObject**.</span><span class="sxs-lookup"><span data-stu-id="e3624-338">hello code is hello same as with hello typed case, but hello method has a different signature since it references an **JsonObject**.</span></span>

```java
mToDoTable
    .delete(insertedItem);
```

<span data-ttu-id="e3624-339">Můžete také odstranit instanci přímo pomocí jeho ID:</span><span class="sxs-lookup"><span data-stu-id="e3624-339">You can also delete an instance directly by using its ID:</span></span>

```java
mToDoTable.delete(ID);
```

### <span data-ttu-id="e3624-340"><a name="json_get"></a>Vrátí všechny řádky z tabulky aplikace bez typu</span><span class="sxs-lookup"><span data-stu-id="e3624-340"><a name="json_get"></a>Return all rows from an untyped table</span></span>
<span data-ttu-id="e3624-341">Následující kód ukazuje, jak Hello tooretrieve celou tabulku.</span><span class="sxs-lookup"><span data-stu-id="e3624-341">hello following code shows how tooretrieve an entire table.</span></span> <span data-ttu-id="e3624-342">Vzhledem k tomu, že používáte JSON tabulky, můžete selektivně načíst jenom některé sloupce tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-342">Since you are using a JSON Table, you can selectively retrieve only some of hello table's columns.</span></span>

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

<span data-ttu-id="e3624-343">Hello stejnou sadu filtrování, filtrování a stránkování metody, které jsou k dispozici pro hello typu modelu jsou k dispozici pro hello bez typu modelu.</span><span class="sxs-lookup"><span data-stu-id="e3624-343">hello same set of filtering, filtering and paging methods that are available for hello typed model are available for hello untyped model.</span></span>

## <span data-ttu-id="e3624-344"><a name="offline-sync"></a>Implementace Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="e3624-344"><a name="offline-sync"></a>Implement Offline Sync</span></span>

<span data-ttu-id="e3624-345">Hello Azure Mobile Apps Client SDK také implementuje offline synchronizace dat s použitím toostore databáze SQLite kopii dat serveru hello místně.</span><span class="sxs-lookup"><span data-stu-id="e3624-345">hello Azure Mobile Apps Client SDK also implements offline synchronization of data by using a SQLite database toostore a copy of hello server data locally.</span></span>  <span data-ttu-id="e3624-346">Operace provedené v offline tabulce nevyžadují toowork mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="e3624-346">Operations performed on an offline table do not require mobile connectivity toowork.</span></span>  <span data-ttu-id="e3624-347">Offline synchronizace je výhodné při odolnost a výkon při hello nákladů na složitější logiku pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="e3624-347">Offline sync aids in resilience and performance at hello expense of more complex logic for conflict resolution.</span></span>  <span data-ttu-id="e3624-348">Hello Azure Mobile Apps Client SDK implementuje hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="e3624-348">hello Azure Mobile Apps Client SDK implements hello following features:</span></span>

* <span data-ttu-id="e3624-349">Přírůstkové synchronizace: Pouze aktualizované a nové záznamy se stáhnou, ukládání spotřebu šířky pásma a paměti.</span><span class="sxs-lookup"><span data-stu-id="e3624-349">Incremental Sync: Only updated and new records are downloaded, saving bandwidth and memory consumption.</span></span>
* <span data-ttu-id="e3624-350">Optimistickou metodu souběžného: Operace se předpokládá, že toosucceed.</span><span class="sxs-lookup"><span data-stu-id="e3624-350">Optimistic Concurrency: Operations are assumed toosucceed.</span></span>  <span data-ttu-id="e3624-351">Řešení konfliktů je odložení, dokud nebude aktualizace se na hello server.</span><span class="sxs-lookup"><span data-stu-id="e3624-351">Conflict Resolution is deferred until updates are performed on hello server.</span></span>
* <span data-ttu-id="e3624-352">Řešení konfliktů: hello SDK zjistí, že změna způsobující konflikt byl na hello server a poskytuje zachytí tooalert hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3624-352">Conflict Resolution: hello SDK detects when a conflicting change has been made at hello server and provides hooks tooalert hello user.</span></span>
* <span data-ttu-id="e3624-353">Obnovitelného odstranění: Odstraněné záznamy jsou označené odstraněné, povolení dalších zařízení tooupdate offline mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e3624-353">Soft Delete: Deleted records are marked deleted, allowing other devices tooupdate their offline cache.</span></span>

### <a name="initialize-offline-sync"></a><span data-ttu-id="e3624-354">Inicializaci Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="e3624-354">Initialize Offline Sync</span></span>

<span data-ttu-id="e3624-355">Každá tabulka offline musí být definován v mezipaměti offline hello před použitím.</span><span class="sxs-lookup"><span data-stu-id="e3624-355">Each offline table must be defined in hello offline cache before use.</span></span>  <span data-ttu-id="e3624-356">Za normálních okolností se okamžitě po vytvoření hello hello klienta provádí definici tabulky:</span><span class="sxs-lookup"><span data-stu-id="e3624-356">Normally, table definition is done immediately after hello creation of hello client:</span></span>

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with hello model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define hello table in hello local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize hello local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-toohello-offline-cache-table"></a><span data-ttu-id="e3624-357">Získat odkaz na toohello Offline tabulky mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e3624-357">Obtain a reference toohello Offline Cache Table</span></span>

<span data-ttu-id="e3624-358">Pro online tabulky, můžete použít `.getTable()`.</span><span class="sxs-lookup"><span data-stu-id="e3624-358">For an online table, you use `.getTable()`.</span></span>  <span data-ttu-id="e3624-359">K offline tabulky, použijte `.getSyncTable()`:</span><span class="sxs-lookup"><span data-stu-id="e3624-359">For an offline table, use `.getSyncTable()`:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

<span data-ttu-id="e3624-360">Všechny metody, které jsou k dispozici pro online tabulky (včetně filtrování, řazení, stránkování, vkládání dat, aktualizace dat a odstraňování dat) pracovat stejně hello i na tabulkách online a offline.</span><span class="sxs-lookup"><span data-stu-id="e3624-360">All hello methods that are available for online tables (including filtering, sorting, paging, inserting data, updating data, and deleting data) work equally well on online and offline tables.</span></span>

### <a name="synchronize-hello-local-offline-cache"></a><span data-ttu-id="e3624-361">Synchronizovat hello místní mezipaměti v režimu Offline</span><span class="sxs-lookup"><span data-stu-id="e3624-361">Synchronize hello Local Offline Cache</span></span>

<span data-ttu-id="e3624-362">Synchronizace je v rámci ovládacího prvku hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-362">Synchronization is within hello control of your app.</span></span>  <span data-ttu-id="e3624-363">Tady je příklad metoda synchronizace:</span><span class="sxs-lookup"><span data-stu-id="e3624-363">Here is an example synchronization method:</span></span>

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

<span data-ttu-id="e3624-364">Pokud název dotazu je zadaný toohello `.pull(query, queryname)` metoda pak přírůstkové synchronizace je použité tooreturn pouze záznamy, které byly vytvořené nebo změněné od vyžadování hello poslední byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="e3624-364">If a query name is provided toohello `.pull(query, queryname)` method, then incremental sync is used tooreturn only records that have been created or changed since hello last successfully completed pull.</span></span>

### <a name="handle-conflicts-during-offline-synchronization"></a><span data-ttu-id="e3624-365">Zpracování konfliktů během Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="e3624-365">Handle Conflicts during Offline Synchronization</span></span>

<span data-ttu-id="e3624-366">Pokud dojde ke konfliktu při `.push()` operace, `MobileServiceConflictException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="e3624-366">If a conflict happens during a `.push()` operation, a `MobileServiceConflictException` is thrown.</span></span>   <span data-ttu-id="e3624-367">Položka server vydal Hello vložené v hello výjimek a může načíst `.getItem()` na hello výjimce.</span><span class="sxs-lookup"><span data-stu-id="e3624-367">hello server-issued item is embedded in hello exception and can be retrieved by `.getItem()` on hello exception.</span></span>  <span data-ttu-id="e3624-368">Upravte hello nabízené pomocí volání hello u objektu MobileServiceSyncContext hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e3624-368">Adjust hello push by calling hello following items on hello MobileServiceSyncContext object:</span></span>

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

<span data-ttu-id="e3624-369">Jakmile se všechny konflikty jsou označené jako nechcete, volání `.push()` znovu tooresolve všechny hello je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="e3624-369">Once all conflicts are marked as you wish, call `.push()` again tooresolve all hello conflicts.</span></span>

## <span data-ttu-id="e3624-370"><a name="custom-api"></a>Volání vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e3624-370"><a name="custom-api"></a>Call a custom API</span></span>

<span data-ttu-id="e3624-371">Vlastní rozhraní API umožňuje toodefine vlastní koncové body, které zveřejňují funkce serveru které není mapování tooan vložení, aktualizaci, odstranění nebo operace čtení.</span><span class="sxs-lookup"><span data-stu-id="e3624-371">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="e3624-372">Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="e3624-372">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="e3624-373">V klientovi aplikace Android volání hello **invokeApi** metoda toocall hello vlastní koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3624-373">From an Android client, you call hello **invokeApi** method toocall hello custom API endpoint.</span></span> <span data-ttu-id="e3624-374">Hello následující příklad ukazuje, jak toocall koncový bod rozhraní API s názvem **completeAll**, který vrátí kolekce třídy s názvem **MarkAllResult**.</span><span class="sxs-lookup"><span data-stu-id="e3624-374">hello following example shows how toocall an API endpoint named **completeAll**, which returns a collection class named **MarkAllResult**.</span></span>

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

<span data-ttu-id="e3624-375">Hello **invokeApi** metoda je volána na hello klienta, který odesílá příspěvku na žádosti toohello nové vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3624-375">hello **invokeApi** method is called on hello client, which sends a POST request toohello new custom API.</span></span> <span data-ttu-id="e3624-376">Hello výsledek vrácený vlastního rozhraní API hello zobrazí v dialogu zprávy, jako jsou všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="e3624-376">hello result returned by hello custom API is displayed in a message dialog, as are any errors.</span></span> <span data-ttu-id="e3624-377">Jiné verze **invokeApi** umožňují volitelně odesílat objekt v textu žádosti hello, zadejte metodu hello HTTP a odeslat parametry dotazu hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e3624-377">Other versions of **invokeApi** let you optionally send an object in hello request body, specify hello HTTP method, and send query parameters with hello request.</span></span> <span data-ttu-id="e3624-378">Netypová verze **invokeApi** k dispozici jsou také.</span><span class="sxs-lookup"><span data-stu-id="e3624-378">Untyped versions of **invokeApi** are provided as well.</span></span>

## <span data-ttu-id="e3624-379"><a name="authentication"></a>Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="e3624-379"><a name="authentication"></a>Add authentication tooyour app</span></span>

<span data-ttu-id="e3624-380">Kurzy již podrobně popisují, jak tooadd tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="e3624-380">Tutorials already describe in detail how tooadd these features.</span></span>

<span data-ttu-id="e3624-381">App Service podporuje [ověřování uživatelů aplikace](app-service-mobile-android-get-started-users.md) pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3624-381">App Service supports [authenticating app users](app-service-mobile-android-get-started-users.md) using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="e3624-382">Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3624-382">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="e3624-383">Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel v váš back-end.</span><span class="sxs-lookup"><span data-stu-id="e3624-383">You can also use hello identity of authenticated users tooimplement authorization rules in your backend.</span></span>

<span data-ttu-id="e3624-384">Jsou podporovány dva ověřování toky: **server** toku a **klienta** toku.</span><span class="sxs-lookup"><span data-stu-id="e3624-384">Two authentication flows are supported: a **server** flow and a **client** flow.</span></span> <span data-ttu-id="e3624-385">Vývojový server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello identity poskytovatelů webové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e3624-385">hello server flow provides hello simplest authentication experience, as it relies on hello identity providers web interface.</span></span>  <span data-ttu-id="e3624-386">Žádné další sady SDK jsou požadované tooimplement serveru tok ověřování.</span><span class="sxs-lookup"><span data-stu-id="e3624-386">No additional SDKs are required tooimplement server flow authentication.</span></span> <span data-ttu-id="e3624-387">Tok ověřování serveru neposkytuje těsná integrace do mobilních zařízení hello a doporučuje se pouze pro testování konceptu scénáře.</span><span class="sxs-lookup"><span data-stu-id="e3624-387">Server flow authentication does not provide a deep integration into hello mobile device and is only recommended for proof of concept scenarios.</span></span>

<span data-ttu-id="e3624-388">tok klienta Hello umožňuje hlubší integrace s funkcí konkrétní zařízení, jako je jednotné přihlašování jako přitom spoléhá na sady SDK poskytované poskytovatelem identity hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-388">hello client flow allows for deeper integration with device-specific capabilities such as single sign-on as it relies on SDKs provided by hello identity provider.</span></span>  <span data-ttu-id="e3624-389">Například můžete integrovat hello Facebook SDK do své mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-389">For example, you can integrate hello Facebook SDK into your mobile application.</span></span>  <span data-ttu-id="e3624-390">Hello mobilního klienta umožňuje přepnout do aplikace Facebook hello a potvrdí, vaše přihlášení před odkládací back tooyour mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-390">hello mobile client swaps into hello Facebook app and confirms your sign-on before swapping back tooyour mobile app.</span></span>

<span data-ttu-id="e3624-391">Čtyři kroky jsou požadované tooenable ověřování ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="e3624-391">Four steps are required tooenable authentication in your app:</span></span>

* <span data-ttu-id="e3624-392">Registrace aplikace pro ověřování pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="e3624-392">Register your app for authentication with an identity provider.</span></span>
* <span data-ttu-id="e3624-393">Konfigurace back-end vaší služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e3624-393">Configure your App Service backend.</span></span>
* <span data-ttu-id="e3624-394">Umožňuje omezte uživatele tooauthenticated tabulky oprávnění pouze na hello back-end služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e3624-394">Restrict table permissions tooauthenticated users only on hello App Service backend.</span></span>
* <span data-ttu-id="e3624-395">Přidáte aplikaci tooyour kód ověřování.</span><span class="sxs-lookup"><span data-stu-id="e3624-395">Add authentication code tooyour app.</span></span>

<span data-ttu-id="e3624-396">Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3624-396">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="e3624-397">Můžete taky hello SID ověřenému uživateli toomodify požadavky.</span><span class="sxs-lookup"><span data-stu-id="e3624-397">You can also use hello SID of an authenticated user toomodify requests.</span></span>  <span data-ttu-id="e3624-398">Další informace najdete v tématu [Začínáme s ověřováním] a hello serveru SDK postupy dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e3624-398">For more information, review [Get started with authentication] and hello Server SDK HOWTO documentation.</span></span>

### <span data-ttu-id="e3624-399"><a name="caching"></a>Ověřování: Tok serveru</span><span class="sxs-lookup"><span data-stu-id="e3624-399"><a name="caching"></a>Authentication: Server Flow</span></span>

<span data-ttu-id="e3624-400">Hello následující kód spustí proces serveru toku přihlášení pomocí zprostředkovatele hello Google.</span><span class="sxs-lookup"><span data-stu-id="e3624-400">hello following code starts a server flow login process using hello Google provider.</span></span>  <span data-ttu-id="e3624-401">Další konfigurace je vyžadován kvůli hello požadavky zabezpečení pro zprostředkovatele Google hello:</span><span class="sxs-lookup"><span data-stu-id="e3624-401">Additional configuration is required because of hello security requirements for hello Google provider:</span></span>

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

<span data-ttu-id="e3624-402">Kromě toho přidejte následující hlavní třída aktivit metoda toohello hello:</span><span class="sxs-lookup"><span data-stu-id="e3624-402">In addition, add hello following method toohello main Activity class:</span></span>

```java
// You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check hello request code matches hello one we send in hello login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check hello error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

<span data-ttu-id="e3624-403">Hello `GOOGLE_LOGIN_REQUEST_CODE` definované v vaší hlavní aktivita se používá pro hello `login()` metoda a v rámci hello `onActivityResult()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-403">hello `GOOGLE_LOGIN_REQUEST_CODE` defined in your main Activity is used for hello `login()` method and within hello `onActivityResult()` method.</span></span>  <span data-ttu-id="e3624-404">Jedinečné číslo, můžete tak dlouho, dokud hello se používá stejné číslo v rámci hello `login()` metoda a hello `onActivityResult()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-404">You can choose any unique number, as long as hello same number is used within hello `login()` method and hello `onActivityResult()` method.</span></span>  <span data-ttu-id="e3624-405">Pokud jste abstraktní hello kód klienta do služby adaptér (jak je uvedeno výše), by měly volat metody odpovídající hello na adaptéru služby hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-405">If you abstract hello client code into a service adapter (as shown earlier), you should call hello appropriate methods on hello service adapter.</span></span>

<span data-ttu-id="e3624-406">Budete také potřebovat pro customtabs tooconfigure hello projektu.</span><span class="sxs-lookup"><span data-stu-id="e3624-406">You also need tooconfigure hello project for customtabs.</span></span>  <span data-ttu-id="e3624-407">Nejdřív zadejte adresu URL přesměrování.</span><span class="sxs-lookup"><span data-stu-id="e3624-407">First specify a redirect-URL.</span></span>  <span data-ttu-id="e3624-408">Přidejte následující fragment kódu příliš hello`AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="e3624-408">Add hello following snippet too`AndroidManifest.xml`:</span></span>

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

<span data-ttu-id="e3624-409">Přidat hello **redirectUriScheme** toohello `build.gradle` souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="e3624-409">Add hello **redirectUriScheme** toohello `build.gradle` file for your application:</span></span>

```text
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

<span data-ttu-id="e3624-410">Nakonec přidejte `com.android.support:customtabs:23.0.1` toohello aplikací v hello `build.gradle` souboru:</span><span class="sxs-lookup"><span data-stu-id="e3624-410">Finally, add `com.android.support:customtabs:23.0.1` toohello dependencies list in hello `build.gradle` file:</span></span>

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.2.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

<span data-ttu-id="e3624-411">Získat ID hello hello přihlášeného uživatele z **MobileServiceUser** pomocí hello **reprezentuje getUserId** metoda.</span><span class="sxs-lookup"><span data-stu-id="e3624-411">Obtain hello ID of hello logged-in user from a **MobileServiceUser** using hello **getUserId** method.</span></span> <span data-ttu-id="e3624-412">Příklad jak toouse Futures toocall hello asynchronní přihlášení rozhraní API, naleznete v části [Začínáme s ověřováním].</span><span class="sxs-lookup"><span data-stu-id="e3624-412">For an example of how toouse Futures toocall hello asynchronous login APIs, see [Get started with authentication].</span></span>

> [!WARNING]
> <span data-ttu-id="e3624-413">Hello schéma adresy URL uvedené rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e3624-413">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="e3624-414">Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="e3624-414">Ensure that all occurrences of `{url_scheme_of_you_app}` match case.</span></span>

### <span data-ttu-id="e3624-415"><a name="caching"></a>Tokeny ověřování do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e3624-415"><a name="caching"></a>Cache authentication tokens</span></span>

<span data-ttu-id="e3624-416">Ukládání do mezipaměti tokeny ověřování vyžaduje toostore hello ID uživatele a ověřovací token místně na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="e3624-416">Caching authentication tokens requires you toostore hello User ID and authentication token locally on hello device.</span></span> <span data-ttu-id="e3624-417">Hello příštím spuštění aplikace hello, můžete zkontrolujte hello mezipaměti, a pokud nejsou tyto hodnoty, můžete přeskočit hello protokolu v postupu a rehydrataci při spotřebě hello klienta s těmito daty.</span><span class="sxs-lookup"><span data-stu-id="e3624-417">hello next time hello app starts, you check hello cache, and if these values are present, you can skip hello log in procedure and rehydrate hello client with this data.</span></span> <span data-ttu-id="e3624-418">Ale tato data jsou citlivé a by měly být uložené šifrována pro zabezpečení v případě, že získá odcizení hello phone.</span><span class="sxs-lookup"><span data-stu-id="e3624-418">However this data is sensitive, and it should be stored encrypted for safety in case hello phone gets stolen.</span></span>  <span data-ttu-id="e3624-419">Zobrazí kompletní příklad, jak jak toocache ověřování tokenů v [mezipaměti část tokeny ověřování][7].</span><span class="sxs-lookup"><span data-stu-id="e3624-419">You can see a complete example of how toocache authentication tokens in [Cache authentication tokens section][7].</span></span>

<span data-ttu-id="e3624-420">Když zkusíte toouse tokenu vypršela platnost, zobrazí se *401 Neautorizováno* odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e3624-420">When you try toouse an expired token, you receive a *401 unauthorized* response.</span></span> <span data-ttu-id="e3624-421">Může zpracovávat ověřování chyb pomocí filtrů.</span><span class="sxs-lookup"><span data-stu-id="e3624-421">You can handle authentication errors using filters.</span></span>  <span data-ttu-id="e3624-422">Filtry intercept požadavky toohello back-end služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e3624-422">Filters intercept requests toohello App Service backend.</span></span> <span data-ttu-id="e3624-423">kód filtru Hello testy hello odpovědi na 401, aktivuje hello přihlašovací proces a potom obnoví hello žádosti, která vygenerovala hello 401.</span><span class="sxs-lookup"><span data-stu-id="e3624-423">hello filter code tests hello response for a 401, triggers hello sign-in process, and then resumes hello request that generated hello 401.</span></span>

### <span data-ttu-id="e3624-424"><a name="refresh"></a>Použití obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="e3624-424"><a name="refresh"></a>Use Refresh Tokens</span></span>

<span data-ttu-id="e3624-425">Hello token vrácený Azure App Service ověřování a autorizace má definovaná životnosti jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3624-425">hello token returned by Azure App Service Authentication and Authorization has a defined life time of one hour.</span></span>  <span data-ttu-id="e3624-426">Po uplynutí této doby musí novému ověření uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="e3624-426">After this period, you must reauthenticate hello user.</span></span>  <span data-ttu-id="e3624-427">Pokud jste dlohotrvající token, který jste obdrželi prostřednictvím ověřování tok klienta a pak můžete novému ověření pomocí Azure App Service ověřování a autorizace pomocí hello stejný token.</span><span class="sxs-lookup"><span data-stu-id="e3624-427">If you are using a long-lived token that you have received via client-flow authentication, then you can reauthenticate with Azure App Service Authentication and Authorization using hello same token.</span></span>  <span data-ttu-id="e3624-428">Další token služby Azure App Service je vytvořen s novou životnost.</span><span class="sxs-lookup"><span data-stu-id="e3624-428">Another Azure App Service token is generated with a new lifetime.</span></span>

<span data-ttu-id="e3624-429">Můžete také registrovat toouse hello poskytovatele aktualizaci tokeny.</span><span class="sxs-lookup"><span data-stu-id="e3624-429">You can also register hello provider toouse Refresh Tokens.</span></span>  <span data-ttu-id="e3624-430">Aktualizovat Token není vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e3624-430">A Refresh Token is not always available.</span></span>  <span data-ttu-id="e3624-431">Je vyžadována další konfigurace:</span><span class="sxs-lookup"><span data-stu-id="e3624-431">Additional configuration is required:</span></span>

* <span data-ttu-id="e3624-432">Pro **Azure Active Directory**, nakonfigurujte tajný klíč klienta pro hello aplikaci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3624-432">For **Azure Active Directory**, configure a client secret for hello Azure Active Directory App.</span></span>  <span data-ttu-id="e3624-433">Zadejte sdílený tajný klíč klienta hello v hello Azure App Service při konfiguraci ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3624-433">Specify hello client secret in hello Azure App Service when configuring Azure Active Directory Authentication.</span></span>  <span data-ttu-id="e3624-434">Při volání metody `.login()`, předat `response_type=code id_token` jako parametr:</span><span class="sxs-lookup"><span data-stu-id="e3624-434">When calling `.login()`, pass `response_type=code id_token` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="e3624-435">Pro **Google**, předat hello `access_type=offline` jako parametr:</span><span class="sxs-lookup"><span data-stu-id="e3624-435">For **Google**, pass hello `access_type=offline` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="e3624-436">Pro **Account Microsoft**, vyberte hello `wl.offline_access` oboru.</span><span class="sxs-lookup"><span data-stu-id="e3624-436">For **Microsoft Account**, select hello `wl.offline_access` scope.</span></span>

<span data-ttu-id="e3624-437">volání toorefresh token, `.refreshUser()`:</span><span class="sxs-lookup"><span data-stu-id="e3624-437">toorefresh a token, call `.refreshUser()`:</span></span>

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

<span data-ttu-id="e3624-438">Jako osvědčený postup vytvoření filtru, který zjistí 401 odpověď ze serveru hello a pokusí toorefresh hello uživatelského tokenu.</span><span class="sxs-lookup"><span data-stu-id="e3624-438">As a best practice, create a filter that detects a 401 response from hello server and tries toorefresh hello user token.</span></span>

## <a name="log-in-with-client-flow-authentication"></a><span data-ttu-id="e3624-439">Přihlaste se pomocí ověřování tok klienta</span><span class="sxs-lookup"><span data-stu-id="e3624-439">Log in with Client-flow Authentication</span></span>

<span data-ttu-id="e3624-440">Obecný postup Hello přihlášení pomocí ověřování tok klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e3624-440">hello general process for logging in with client-flow authentication is as follows:</span></span>

* <span data-ttu-id="e3624-441">Konfigurace Azure App Service ověřování a autorizaci, stejně jako server tok ověřování.</span><span class="sxs-lookup"><span data-stu-id="e3624-441">Configure Azure App Service Authentication and Authorization as you would server-flow authentication.</span></span>
* <span data-ttu-id="e3624-442">Integrate zprostředkovatele ověřování hello SDK pro ověřování tooproduce přístupový token.</span><span class="sxs-lookup"><span data-stu-id="e3624-442">Integrate hello authentication provider SDK for authentication tooproduce an access token.</span></span>
* <span data-ttu-id="e3624-443">Volání hello `.login()` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e3624-443">Call hello `.login()` method as follows:</span></span>

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

<span data-ttu-id="e3624-444">Nahraďte hello `onSuccess()` metoda s ať vám kód chcete toouse na úspěšného přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e3624-444">Replace hello `onSuccess()` method with whatever code you wish toouse on a successful login.</span></span>  <span data-ttu-id="e3624-445">Hello `{provider}` řetězec je platný zprostředkovatel: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, nebo **twitter**.</span><span class="sxs-lookup"><span data-stu-id="e3624-445">hello `{provider}` string is a valid provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, or **twitter**.</span></span>  <span data-ttu-id="e3624-446">Pokud jste implementovali vlastní ověřování, můžete také použít hello vlastního ověřování zprostředkovatele značky.</span><span class="sxs-lookup"><span data-stu-id="e3624-446">If you have implemented custom authentication, then you can also use hello custom authentication provider tag.</span></span>

### <span data-ttu-id="e3624-447"><a name="adal"></a>Ověřuje uživatele pomocí hello Active Directory Authentication Library (ADAL)</span><span class="sxs-lookup"><span data-stu-id="e3624-447"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library (ADAL)</span></span>

<span data-ttu-id="e3624-448">Můžete vytvořit hello Active Directory Authentication Library (ADAL) toosign uživatelů do vaší aplikace pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3624-448">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="e3624-449">Pomocí přihlášení toku klienta je často vhodnější toousing hello `loginAsync()` metody jak poskytuje více nativní UX chování a umožňuje pro další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="e3624-449">Using a client flow login is often preferable toousing hello `loginAsync()` methods as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="e3624-450">Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] [ 22] kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3624-450">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][22] tutorial.</span></span> <span data-ttu-id="e3624-451">Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3624-451">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="e3624-452">Nainstalujte ADAL změnou vaší hello tooinclude souboru build.gradle následující definice:</span><span class="sxs-lookup"><span data-stu-id="e3624-452">Install ADAL by modifying your build.gradle file tooinclude hello following definitions:</span></span>

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. <span data-ttu-id="e3624-453">Přidejte následující kód tooyour aplikace, která hello následující náhrady hello:</span><span class="sxs-lookup"><span data-stu-id="e3624-453">Add hello following code tooyour application, making hello following replacements:</span></span>

* <span data-ttu-id="e3624-454">Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-454">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="e3624-455">Hello formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="e3624-455">hello format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span>
* <span data-ttu-id="e3624-456">Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3624-456">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="e3624-457">ID klienta hello můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="e3624-457">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
* <span data-ttu-id="e3624-458">Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3624-458">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
* <span data-ttu-id="e3624-459">Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e3624-459">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="e3624-460">Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="e3624-460">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <span data-ttu-id="e3624-461"><a name="filters"></a>Upravit hello komunikaci klienta se serverem</span><span class="sxs-lookup"><span data-stu-id="e3624-461"><a name="filters"></a>Adjust hello Client-Server Communication</span></span>

<span data-ttu-id="e3624-462">Hello připojení klienta je obvykle základní připojení HTTP pomocí hello základní knihovna HTTP dodává s hello sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="e3624-462">hello Client connection is normally a basic HTTP connection using hello underlying HTTP library supplied with hello Android SDK.</span></span>  <span data-ttu-id="e3624-463">Tady je několik důvodů, proč byste měli toochange který:</span><span class="sxs-lookup"><span data-stu-id="e3624-463">There are several reasons why you would want toochange that:</span></span>

* <span data-ttu-id="e3624-464">Chcete toouse alternativní HTTP knihovny tooadjust vypršení časových limitů.</span><span class="sxs-lookup"><span data-stu-id="e3624-464">You wish toouse an alternate HTTP library tooadjust timeouts.</span></span>
* <span data-ttu-id="e3624-465">Chcete tooprovide indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="e3624-465">You wish tooprovide a progress bar.</span></span>
* <span data-ttu-id="e3624-466">Chcete tooadd funkce správy toosupport rozhraní API vlastní hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e3624-466">You wish tooadd a custom header toosupport API management functionality.</span></span>
* <span data-ttu-id="e3624-467">Chcete toointercept neúspěšných odpovědí proto, že můžete implementovat opětovné ověření.</span><span class="sxs-lookup"><span data-stu-id="e3624-467">You wish toointercept a failed response so that you can implement reauthentication.</span></span>
* <span data-ttu-id="e3624-468">Chcete toolog back-end požadavky tooan analytics službu.</span><span class="sxs-lookup"><span data-stu-id="e3624-468">You wish toolog backend requests tooan analytics service.</span></span>

### <a name="using-an-alternate-http-library"></a><span data-ttu-id="e3624-469">Pomocí alternativní knihovny HTTP</span><span class="sxs-lookup"><span data-stu-id="e3624-469">Using an alternate HTTP Library</span></span>

<span data-ttu-id="e3624-470">Volání hello `.setAndroidHttpClientFactory()` metoda ihned po vytvoření odkaz na klienta.</span><span class="sxs-lookup"><span data-stu-id="e3624-470">Call hello `.setAndroidHttpClientFactory()` method immediately after creating your client reference.</span></span>  <span data-ttu-id="e3624-471">Například tooset hello připojení časový limit too60 v sekundách (namísto výchozí hello 10 sekund):</span><span class="sxs-lookup"><span data-stu-id="e3624-471">For example, tooset hello connection timeout too60 seconds (instead of hello default 10 seconds):</span></span>

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a><span data-ttu-id="e3624-472">Implementace filtru průběh</span><span class="sxs-lookup"><span data-stu-id="e3624-472">Implement a Progress Filter</span></span>

<span data-ttu-id="e3624-473">Zachycení každou žádost můžete implementovat implementací `ServiceFilter`.</span><span class="sxs-lookup"><span data-stu-id="e3624-473">You can implement an intercept of every request by implementing a `ServiceFilter`.</span></span>  <span data-ttu-id="e3624-474">Například následující hello aktualizací indikátor průběhu předem vytvořené:</span><span class="sxs-lookup"><span data-stu-id="e3624-474">For example, hello following updates a pre-created progress bar:</span></span>

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    pubic void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

<span data-ttu-id="e3624-475">Tento klient toohello filtru můžete připojit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e3624-475">You can attach this filter toohello client as follows:</span></span>

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a><span data-ttu-id="e3624-476">Přizpůsobení hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="e3624-476">Customize Request Headers</span></span>

<span data-ttu-id="e3624-477">Použijte hello `ServiceFilter` a připojte hello filtru v hello stejným způsobem jako hello `ProgressFilter`:</span><span class="sxs-lookup"><span data-stu-id="e3624-477">Use hello following `ServiceFilter` and attach hello filter in hello same way as hello `ProgressFilter`:</span></span>

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <span data-ttu-id="e3624-478"><a name="conversions"></a>Konfigurace automatického serializace</span><span class="sxs-lookup"><span data-stu-id="e3624-478"><a name="conversions"></a>Configure Automatic Serialization</span></span>

<span data-ttu-id="e3624-479">Můžete zadat převod strategie, která se použije tooevery sloupce s použitím hello [gson] [ 3] rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3624-479">You can specify a conversion strategy that applies tooevery column by using hello [gson][3] API.</span></span> <span data-ttu-id="e3624-480">Hello Android Klientská knihovna používá [gson] [ 3] pozadí hello tooserialize Java objekty tooJSON dat před odesláním dat hello tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e3624-480">hello Android client library uses [gson][3] behind hello scenes tooserialize Java objects tooJSON data before hello data is sent tooAzure App Service.</span></span>  <span data-ttu-id="e3624-481">Hello následující kód používá hello **setFieldNamingStrategy()** metoda tooset hello strategie.</span><span class="sxs-lookup"><span data-stu-id="e3624-481">hello following code uses hello **setFieldNamingStrategy()** method tooset hello strategy.</span></span> <span data-ttu-id="e3624-482">Tento příklad odstraní hello počáteční znak ("m") a pak malá hello další znak, pro každý název pole.</span><span class="sxs-lookup"><span data-stu-id="e3624-482">This example will delete hello initial character (an "m"), and then lower-case hello next character, for every field name.</span></span> <span data-ttu-id="e3624-483">Například ho by zapnout "střední" do "id".</span><span class="sxs-lookup"><span data-stu-id="e3624-483">For example, it would turn "mId" into "id."</span></span>  <span data-ttu-id="e3624-484">Implementace převod strategie tooreduce hello nutnost `SerializedName()` poznámky na většina polí.</span><span class="sxs-lookup"><span data-stu-id="e3624-484">Implement a conversion strategy tooreduce hello need for `SerializedName()` annotations on most fields.</span></span>

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStategy)
);
```

<span data-ttu-id="e3624-485">Tento kód musí být spuštěn před vytvořením mobilního klienta odkaz pomocí hello **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="e3624-485">This code must be executed before creating a mobile client reference using hello **MobileServiceClient**.</span></span>

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Začínáme s ověřováním]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
