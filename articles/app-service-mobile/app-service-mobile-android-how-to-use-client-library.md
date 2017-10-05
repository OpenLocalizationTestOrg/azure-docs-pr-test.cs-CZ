---
title: "Jak používat Azure Mobile Apps SDK pro Android | Microsoft Docs"
description: "Jak používat Azure Mobile Apps SDK pro Android"
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
ms.openlocfilehash: 4b15d024ca6d5bbafe83d321a64021aecd78c4a8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a><span data-ttu-id="0d2e6-103">Jak používat Azure Mobile Apps SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="0d2e6-103">How to use the Azure Mobile Apps SDK for Android</span></span>

<span data-ttu-id="0d2e6-104">Tento průvodce vám ukáže, jak používat klienta Android SDK pro Mobile Apps k implementaci běžné scénáře, jako například:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-104">This guide shows you how to use the Android client SDK for Mobile Apps to implement common scenarios, such as:</span></span>

* <span data-ttu-id="0d2e6-105">Dotazování na data (vložení, aktualizace a odstranění).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-105">Querying for data (inserting, updating, and deleting).</span></span>
* <span data-ttu-id="0d2e6-106">Ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-106">Authentication.</span></span>
* <span data-ttu-id="0d2e6-107">Zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-107">Handling errors.</span></span>
* <span data-ttu-id="0d2e6-108">Vlastní nastavení klienta.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-108">Customizing the client.</span></span>

<span data-ttu-id="0d2e6-109">Tato příručka se zaměřuje na straně klienta SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-109">This guide focuses on the client-side Android SDK.</span></span>  <span data-ttu-id="0d2e6-110">Další informace o serverové sady SDK pro Mobile Apps naleznete v tématu [pracovat s .NET back-end SDK] [ 10] nebo [použití back-end Node.js SDK][11].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-110">To learn more about the server-side SDKs for Mobile Apps, see [Work with .NET backend SDK][10] or [How to use the Node.js backend SDK][11].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="0d2e6-111">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-111">Reference Documentation</span></span>

<span data-ttu-id="0d2e6-112">Můžete najít [referenční dokumentace rozhraní API Javadocs] [ 12] Android klientské knihovny na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-112">You can find the [Javadocs API reference][12] for the Android client library on GitHub.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0d2e6-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="0d2e6-113">Supported Platforms</span></span>

<span data-ttu-id="0d2e6-114">Azure Mobile Apps SDK pro Android podporuje rozhraní API úrovně 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát) pro telefon i tablet velikostem.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-114">The Azure Mobile Apps SDK for Android supports API levels 19 through 24 (KitKat through Nougat) for phone and tablet form factors.</span></span>  <span data-ttu-id="0d2e6-115">Ověřování, zejména využívá běžně webové framework získat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-115">Authentication, in particular, utilizes a common web framework approach to gather credentials.</span></span>  <span data-ttu-id="0d2e6-116">Ověřování serveru toku nefunguje s malé formuláře Multi-Factor zařízení, jako jsou sleduje.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-116">Server-flow authentication does not work with small form factor devices such as watches.</span></span>

## <a name="setup-and-prerequisites"></a><span data-ttu-id="0d2e6-117">Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="0d2e6-117">Setup and Prerequisites</span></span>

<span data-ttu-id="0d2e6-118">Dokončení [využít postup rychlého spuštění Mobile Apps](app-service-mobile-android-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-118">Complete the [Mobile Apps quickstart](app-service-mobile-android-get-started.md) tutorial.</span></span>  <span data-ttu-id="0d2e6-119">Tato úloha zajistí, že byly splněny všechny požadavky pro Azure Mobile Apps pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-119">This task ensures all pre-requisites for developing Azure Mobile Apps have been met.</span></span>  <span data-ttu-id="0d2e6-120">Rychlý Start také vám pomůže nakonfigurovat svůj účet a vytvoření vaší první back-endu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-120">The Quickstart also helps you configure your account and create your first Mobile App backend.</span></span>

<span data-ttu-id="0d2e6-121">Pokud se rozhodnete není k dokončení tohoto kurzu rychlý start, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-121">If you decide not to complete the Quickstart tutorial, complete the following tasks:</span></span>

* <span data-ttu-id="0d2e6-122">[Vytvořte back-end mobilní aplikace] [ 13] pro použití s aplikací systému Android.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-122">[create a Mobile App backend][13] to use with your Android app.</span></span>
* <span data-ttu-id="0d2e6-123">V nástroji Android Studio [aktualizace Gradle sestavení souborů](#gradle-build).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-123">In Android Studio, [update the Gradle build files](#gradle-build).</span></span>
* <span data-ttu-id="0d2e6-124">[Povolit oprávnění internet](#enable-internet).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-124">[Enable internet permission](#enable-internet).</span></span>

### <span data-ttu-id="0d2e6-125"><a name="gradle-build"></a>Aktualizovat soubor sestavení Gradle</span><span class="sxs-lookup"><span data-stu-id="0d2e6-125"><a name="gradle-build"></a>Update the Gradle build file</span></span>

<span data-ttu-id="0d2e6-126">Obě změnit **build.gradle** soubory:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-126">Change both **build.gradle** files:</span></span>

1. <span data-ttu-id="0d2e6-127">Přidejte tento kód, který *projektu* úroveň **build.gradle** souboru uvnitř *buildscript* značky:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-127">Add this code to the *Project* level **build.gradle** file inside the *buildscript* tag:</span></span>

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. <span data-ttu-id="0d2e6-128">Přidejte tento kód, který *modulu aplikace* úroveň **build.gradle** souboru uvnitř *závislosti* značky:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-128">Add this code to the *Module app* level **build.gradle** file inside the *dependencies* tag:</span></span>

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    <span data-ttu-id="0d2e6-129">Nejnovější verze je aktuálně 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-129">Currently the latest version is 3.3.0.</span></span> <span data-ttu-id="0d2e6-130">Podporované verze jsou uvedeny [na panelu koše][14].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-130">The supported versions are listed [on bintray][14].</span></span>

### <span data-ttu-id="0d2e6-131"><a name="enable-internet"></a>Povolit oprávnění internet</span><span class="sxs-lookup"><span data-stu-id="0d2e6-131"><a name="enable-internet"></a>Enable internet permission</span></span>

<span data-ttu-id="0d2e6-132">Pro přístup k Azure, vaše aplikace musí mít povolené oprávnění Internetu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-132">To access Azure, your app must have the INTERNET permission enabled.</span></span> <span data-ttu-id="0d2e6-133">Pokud ještě není povolené, přidejte následující řádek kódu do vaší **AndroidManifest.xml** souboru:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-133">If it's not already enabled, add the following line of code to your **AndroidManifest.xml** file:</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a><span data-ttu-id="0d2e6-134">Umožňuje vytvořit připojení klienta</span><span class="sxs-lookup"><span data-stu-id="0d2e6-134">Create a Client Connection</span></span>

<span data-ttu-id="0d2e6-135">Azure Mobile Apps poskytuje čtyři funkce pro mobilní aplikace:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-135">Azure Mobile Apps provides four functions to your mobile application:</span></span>

* <span data-ttu-id="0d2e6-136">Přístup k datům a Offline synchronizace s služby mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-136">Data Access and Offline Synchronization with an Azure Mobile Apps Service.</span></span>
* <span data-ttu-id="0d2e6-137">Volání rozhraní API vlastní napsané pomocí Azure Mobile Apps Server SDK.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-137">Call Custom APIs written with the Azure Mobile Apps Server SDK.</span></span>
* <span data-ttu-id="0d2e6-138">Ověřování pomocí služby Azure App Service ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-138">Authentication with Azure App Service Authentication and Authorization.</span></span>
* <span data-ttu-id="0d2e6-139">Registrace nabízených oznámení v Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-139">Push Notification Registration with Notification Hubs.</span></span>

<span data-ttu-id="0d2e6-140">Každá z těchto funkcí vyžaduje nejprve vytvoření `MobileServiceClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-140">Each of these functions first requires that you create a `MobileServiceClient` object.</span></span>  <span data-ttu-id="0d2e6-141">Pouze jeden `MobileServiceClient` objekt by měl být vytvořen v rámci vašeho mobilního klienta (to znamená, musí být Singleton vzor).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-141">Only one `MobileServiceClient` object should be created within your mobile client (that is, it should be a Singleton pattern).</span></span>  <span data-ttu-id="0d2e6-142">Chcete-li vytvořit `MobileServiceClient` objektu:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-142">To create a `MobileServiceClient` object:</span></span>

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

<span data-ttu-id="0d2e6-143">`<MobileAppUrl>` Je řetězec nebo objekt adresy URL, která odkazuje na vaše mobilní back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-143">The `<MobileAppUrl>` is either a string or a URL object that points to your mobile backend.</span></span>  <span data-ttu-id="0d2e6-144">Pokud používáte Azure App Service k hostování vaší mobilní back-end, pak se ujistěte, můžete použít zabezpečené `https://` verze adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-144">If you are using Azure App Service to host your mobile backend, then ensure you use the secure `https://` version of the URL.</span></span>

<span data-ttu-id="0d2e6-145">Klient taky vyžaduje přístup k aktivity nebo kontextu - `this` parametr v příkladu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-145">The client also requires access to the Activity or Context - the `this` parameter in the example.</span></span>  <span data-ttu-id="0d2e6-146">Měly by během proběhnout konstrukce MobileServiceClient `onCreate()` metoda aktivity odkazuje v `AndroidManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-146">The MobileServiceClient construction should happen within the `onCreate()` method of the Activity referenced in the `AndroidManifest.xml` file.</span></span>

<span data-ttu-id="0d2e6-147">Jako osvědčený postup by měl abstraktní komunikace serveru do vlastní třídy (singleton-pattern).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-147">As a best practice, you should abstract server communication into its own (singleton-pattern) class.</span></span>  <span data-ttu-id="0d2e6-148">V takovém případě by měla předávat aktivitu v rámci konstruktoru správně nakonfigurovat službu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-148">In this case, you should pass the Activity within the constructor to appropriately configure the service.</span></span>  <span data-ttu-id="0d2e6-149">Například:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-149">For example:</span></span>

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

<span data-ttu-id="0d2e6-150">Nyní můžete volat `AzureServiceAdapter.Initialize(this);` v `onCreate()` metoda hlavní činnosti.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-150">You can now call `AzureServiceAdapter.Initialize(this);` in the `onCreate()` method of your main activity.</span></span>  <span data-ttu-id="0d2e6-151">Žádné jiné metody potřebovali mít přístup k použití klienta `AzureServiceAdapter.getInstance();` získat odkaz na službu adaptéru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-151">Any other methods needing access to the client use `AzureServiceAdapter.getInstance();` to obtain a reference to the service adapter.</span></span>

## <a name="data-operations"></a><span data-ttu-id="0d2e6-152">Operace s daty</span><span class="sxs-lookup"><span data-stu-id="0d2e6-152">Data Operations</span></span>

<span data-ttu-id="0d2e6-153">Základní sady Azure Mobile Apps SDK je poskytnout přístup k datům uloženým v rámci SQL Azure na back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-153">The core of the Azure Mobile Apps SDK is to provide access to data stored within SQL Azure on the Mobile App backend.</span></span>  <span data-ttu-id="0d2e6-154">Můžete přístup k těmto datům pomocí silného typu třídy (doporučeno) nebo netypová dotazy (nedoporučuje se).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-154">You can access this data using strongly typed classes (preferred) or untyped queries (not recommended).</span></span>  <span data-ttu-id="0d2e6-155">Hromadným Tato část se zabývá pomocí třídy silného typu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-155">The bulk of this section deals with using strongly typed classes.</span></span>

### <a name="define-client-data-classes"></a><span data-ttu-id="0d2e6-156">Definování datových tříd, klienta</span><span class="sxs-lookup"><span data-stu-id="0d2e6-156">Define client data classes</span></span>

<span data-ttu-id="0d2e6-157">Pro přístup k datům z tabulek SQL Azure, definujte klienta datových tříd, které odpovídají na tabulky v back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-157">To access data from SQL Azure tables, define client data classes that correspond to the tables in the Mobile App backend.</span></span> <span data-ttu-id="0d2e6-158">Příklady v tomto tématu předpokládat tabulku s názvem **MyDataTable**, který má následující sloupce:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-158">Examples in this topic assume a table named **MyDataTable**, which has the following columns:</span></span>

* <span data-ttu-id="0d2e6-159">id</span><span class="sxs-lookup"><span data-stu-id="0d2e6-159">id</span></span>
* <span data-ttu-id="0d2e6-160">Text</span><span class="sxs-lookup"><span data-stu-id="0d2e6-160">text</span></span>
* <span data-ttu-id="0d2e6-161">Dokončení</span><span class="sxs-lookup"><span data-stu-id="0d2e6-161">complete</span></span>

<span data-ttu-id="0d2e6-162">Objekt odpovídající typu klienta se nachází v souboru s názvem **MyDataTable.java**:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-162">The corresponding typed client-side object resides in a file called **MyDataTable.java**:</span></span>

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

<span data-ttu-id="0d2e6-163">Přidejte metody getter a setter metody pro každé pole, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-163">Add getter and setter methods for each field that you add.</span></span>  <span data-ttu-id="0d2e6-164">Pokud SQL Azure tabulka obsahuje více sloupců, přidáte by odpovídající pole pro tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-164">If your SQL Azure table contains more columns, you would add the corresponding fields to this class.</span></span>  <span data-ttu-id="0d2e6-165">Například pokud DTO sloupec s prioritou celé číslo bylo (objekt přenosu dat) a pak může přidejte toto pole, společně s její metody getter a setter metody:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-165">For example, if the DTO (data transfer object) had an integer Priority column, then you might add this field, along with its getter and setter methods:</span></span>

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

<span data-ttu-id="0d2e6-166">Další postup vytvoření dalších tabulek v váš back-end Mobile Apps naleznete v tématu [postupy: definování řadič tabulky] [ 15] (.NET back-end) nebo [definovat tabulky pomocí dynamické schématu] [ 16] (back-end Node.js).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-166">To learn how to create additional tables in your Mobile Apps backend, see [How to: Define a table controller][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js backend).</span></span>

<span data-ttu-id="0d2e6-167">Tabulce back-end Azure Mobile Apps definuje pět speciální pole, čtyři, které jsou dostupné klientům:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-167">An Azure Mobile Apps backend table defines five special fields, four of which are available to clients:</span></span>

* <span data-ttu-id="0d2e6-168">`String id`: Globálně jedinečné ID záznamu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-168">`String id`: The globally unique ID for the record.</span></span>  <span data-ttu-id="0d2e6-169">Jako osvědčený postup, ujistěte se, id řetězcovou reprezentaci [UUID] [ 17] objektu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-169">As a best practice, make the id the String representation of a [UUID][17] object.</span></span>
* <span data-ttu-id="0d2e6-170">`DateTimeOffset updatedAt`: Datum a čas poslední aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-170">`DateTimeOffset updatedAt`: The date/time of the last update.</span></span>  <span data-ttu-id="0d2e6-171">Pole updatedAt nastavena serverem a musí být nastavena nikdy váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-171">The updatedAt field is set by the server and should never be set by your client code.</span></span>
* <span data-ttu-id="0d2e6-172">`DateTimeOffset createdAt`: Datum a čas vytvořený objekt.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-172">`DateTimeOffset createdAt`: The date/time that the object was created.</span></span>  <span data-ttu-id="0d2e6-173">Pole createdAt nastavena serverem a musí být nastavena nikdy váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-173">The createdAt field is set by the server and should never be set by your client code.</span></span>
* <span data-ttu-id="0d2e6-174">`byte[] version`: Obvykle vyjádřený jako řetězec, verze je také nastavená serverem.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-174">`byte[] version`: Normally represented as a string, the version is also set by the server.</span></span>
* <span data-ttu-id="0d2e6-175">`boolean deleted`: Označuje, že záznam má byla odstraněna ale ještě nebyla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-175">`boolean deleted`: Indicates that the record has been deleted but not purged yet.</span></span>  <span data-ttu-id="0d2e6-176">Nepoužívejte `deleted` jako vlastnost v třídě.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-176">Do not use `deleted` as a property in your class.</span></span>

<span data-ttu-id="0d2e6-177">`id` Pole je povinné.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-177">The `id` field is required.</span></span>  <span data-ttu-id="0d2e6-178">`updatedAt` Pole a `version` pole se používají pro offline synchronizace (pro přírůstkové synchronizace a dojde ke konfliktu řešení v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-178">The `updatedAt` field and `version` field are used for offline synchronization (for incremental sync and conflict resolution respectively).</span></span>  <span data-ttu-id="0d2e6-179">`createdAt` Pole je odkaz na pole a není používán klienta.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-179">The `createdAt` field is a reference field and is not used by the client.</span></span>  <span data-ttu-id="0d2e6-180">Názvy jsou názvy "přes přenosu" vlastnosti a nejsou upravit.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-180">The names are "across-the-wire" names of the properties and are not adjustable.</span></span>  <span data-ttu-id="0d2e6-181">Můžete však vytvořit mapování mezi objektu a názvy "přes přenosu" pomocí [gson] [ 3] knihovny.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-181">However, you can create a mapping between your object and the "across-the-wire" names using the [gson][3] library.</span></span>  <span data-ttu-id="0d2e6-182">Například:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-182">For example:</span></span>

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

### <a name="create-a-table-reference"></a><span data-ttu-id="0d2e6-183">Vytvořit odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="0d2e6-183">Create a Table Reference</span></span>

<span data-ttu-id="0d2e6-184">Chcete-li získat přístup k tabulce, nejprve vytvořit [MobileServiceTable] [ 8] objekt voláním **jít** metodu [MobileServiceClient][9].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-184">To access a table, first create a [MobileServiceTable][8] object by calling the **getTable** method on the [MobileServiceClient][9].</span></span>  <span data-ttu-id="0d2e6-185">Tato metoda má dva přetížení:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-185">This method has two overloads:</span></span>

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

<span data-ttu-id="0d2e6-186">V následujícím kódu **mClient** je odkaz na MobileServiceClient objektu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-186">In the following code, **mClient** is a reference to your MobileServiceClient object.</span></span>  <span data-ttu-id="0d2e6-187">První přetížení se používá, kde název třídy a název tabulky jsou stejné, a ten, používá se v rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-187">The first overload is used where the class name and the table name are the same, and is the one used in the Quickstart:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

<span data-ttu-id="0d2e6-188">Druhý přetížení se používá při liší od názvu třídy název tabulky: první parametr je název tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-188">The second overload is used when the table name is different from the class name: the first parameter is the table name.</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <span data-ttu-id="0d2e6-189"><a name="query"></a>Dotazování tabulky back-end</span><span class="sxs-lookup"><span data-stu-id="0d2e6-189"><a name="query"></a>Query a Backend Table</span></span>

<span data-ttu-id="0d2e6-190">Nejprve získejte odkaz na tabulku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-190">First, obtain a table reference.</span></span>  <span data-ttu-id="0d2e6-191">Potom spusťte dotaz na odkaz na tabulku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-191">Then execute a query on the table reference.</span></span>  <span data-ttu-id="0d2e6-192">Dotaz je libovolnou kombinaci:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-192">A query is any combination of:</span></span>

* <span data-ttu-id="0d2e6-193">A `.where()` [klauzuli filtru](#filtering).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-193">A `.where()` [filter clause](#filtering).</span></span>
* <span data-ttu-id="0d2e6-194">`.orderBy()` [Řazení klauzule](#sorting).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-194">An `.orderBy()` [ordering clause](#sorting).</span></span>
* <span data-ttu-id="0d2e6-195">A `.select()` [klauzule výběr pole](#selection).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-195">A `.select()` [field selection clause](#selection).</span></span>
* <span data-ttu-id="0d2e6-196">A `.skip()` a `.top()` pro [stránkovaného výsledky](#paging).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-196">A `.skip()` and `.top()` for [paged results](#paging).</span></span>

<span data-ttu-id="0d2e6-197">Klauzulích musí být uvedené v předchozí.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-197">The clauses must be presented in the preceding order.</span></span>

### <span data-ttu-id="0d2e6-198"><a name="filter"></a>Filtrování výsledků</span><span class="sxs-lookup"><span data-stu-id="0d2e6-198"><a name="filter"></a> Filtering Results</span></span>

<span data-ttu-id="0d2e6-199">Obecná forma dotazu je:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-199">The general form of a query is:</span></span>

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

<span data-ttu-id="0d2e6-200">V předchozím příkladu vrací všechny výsledky (až maximální velikost stránky nastavená serverem).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-200">The preceding example returns all results (up to the maximum page size set by the server).</span></span>  <span data-ttu-id="0d2e6-201">`.execute()` Metoda provede daný dotaz na back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-201">The `.execute()` method executes the query on the backend.</span></span>  <span data-ttu-id="0d2e6-202">Dotaz je převést na [OData v3] [ 19] Dotázat se před přenosem na back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-202">The query is converted to an [OData v3][19] query before transmission to the Mobile Apps backend.</span></span>  <span data-ttu-id="0d2e6-203">Back-end mobilní aplikace na přijetí, převede dotaz na příkazu SQL před provedením v instanci SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-203">On receipt, the Mobile Apps backend converts the query into an SQL statement before executing it on the SQL Azure instance.</span></span>  <span data-ttu-id="0d2e6-204">Vzhledem k tomu, že nějakou dobu, trvá síťové aktivity `.execute()` metoda vrátí [ `ListenableFuture<E>` ] [ 18].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-204">Since network activity takes some time, The `.execute()` method returns a [`ListenableFuture<E>`][18].</span></span>

### <span data-ttu-id="0d2e6-205"><a name="filtering"></a>Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="0d2e6-205"><a name="filtering"></a>Filter returned data</span></span>

<span data-ttu-id="0d2e6-206">Provádění následující dotaz vrátí všechny položky z **ToDoItem** tabulky kde **dokončení** rovná **false**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-206">The following query execution returns all items from the **ToDoItem** table where **complete** equals **false**.</span></span>

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-207">**mToDoTable** se odkaz na tabulku mobilní službu, která jsme vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-207">**mToDoTable** is the reference to the mobile service table that we created previously.</span></span>

<span data-ttu-id="0d2e6-208">Definujte filtr pomocí **kde** volání metody na odkaz na tabulku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-208">Define a filter using the **where** method call on the table reference.</span></span> <span data-ttu-id="0d2e6-209">**Kde** metoda následuje **pole** metoda následuje metodu, která určuje logické predikátu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-209">The **where** method is followed by a **field** method followed by a method that specifies the logical predicate.</span></span> <span data-ttu-id="0d2e6-210">Zahrnout možných metod predikátem **eq** (rovná), **ne** (nerovná), **gt** (větší než), **ge** (větší než nebo rovno), **lt** (méně než), **le** (je menší než nebo rovno).</span><span class="sxs-lookup"><span data-stu-id="0d2e6-210">Possible predicate methods include **eq** (equals), **ne** (not equal), **gt** (greater than), **ge** (greater than or equal to), **lt** (less than), **le** (less than or equal to).</span></span> <span data-ttu-id="0d2e6-211">Tyto metody umožňují porovnat řetězec a číslo pole na konkrétní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-211">These methods let you compare number and string fields to specific values.</span></span>

<span data-ttu-id="0d2e6-212">Můžete filtrovat podle data.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-212">You can filter on dates.</span></span> <span data-ttu-id="0d2e6-213">Následující metody umožňují porovnat pole pro celý datum nebo částí data: **roku**, **měsíc**, **den**, **hodinu**, **minutu**, a **druhý**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-213">The following methods let you compare the entire date field or parts of the date: **year**, **month**, **day**, **hour**, **minute**, and **second**.</span></span> <span data-ttu-id="0d2e6-214">Následující příklad přidá filtr pro položky jejichž *datum splatnosti* rovná 2013.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-214">The following example adds a filter for items whose *due date* equals 2013.</span></span>

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-215">Následující metody podporují komplexní filtry na polí s řetězcem: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **nahradit**, **toLower**, **toUpper**, **trim**, a **délka**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-215">The following methods support complex filters on string fields: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, and **length**.</span></span> <span data-ttu-id="0d2e6-216">Následující příklad filtry pro tabulku řádků, kde *text* sloupec začíná "PRI0."</span><span class="sxs-lookup"><span data-stu-id="0d2e6-216">The following example filters for table rows where the *text* column starts with "PRI0."</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-217">Následující operátor metody jsou podporovány na pole s počtem: **přidat**, **sub**, **mul**, **div**, **mod**, **podlaží**, **mezní hodnoty**, a **ZAOKROUHLIT**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-217">The following operator methods are supported on number fields: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**, and **round**.</span></span> <span data-ttu-id="0d2e6-218">Následující příklad filtry pro tabulku řádků, kde **doba trvání** je číslo sudé.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-218">The following example filters for table rows where the **duration** is an even number.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-219">Predikáty můžete kombinovat s tyto logické metody: **a**, **nebo** a **není**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-219">You can combine predicates with these logical methods: **and**, **or** and **not**.</span></span> <span data-ttu-id="0d2e6-220">Následující příklad kombinuje dva v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-220">The following example combines two of the preceding examples.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-221">Skupiny a vnořit logické operátory:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-221">Group and nest logical operators:</span></span>

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

<span data-ttu-id="0d2e6-222">Podrobnější diskuzi a příklady filtrování, najdete v části [zkoumat bohatost modelu dotazu Android klienta][20].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-222">For more detailed discussion and examples of filtering, see [Exploring the richness of the Android client query model][20].</span></span>

### <span data-ttu-id="0d2e6-223"><a name="sorting"></a>Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="0d2e6-223"><a name="sorting"></a>Sort returned data</span></span>

<span data-ttu-id="0d2e6-224">Následující kód vrátí všechny položky z tabulky **ToDoItems** seřadit vzestupně podle *text* pole.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-224">The following code returns all items from a table of **ToDoItems** sorted ascending by the *text* field.</span></span> <span data-ttu-id="0d2e6-225">*mToDoTable* se odkaz na tabulku back-end, který jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-225">*mToDoTable* is the reference to the backend table that you created previously:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-226">První parametr **orderBy** metoda je stejný jako název pole, ve kterém se má seřadit řetězec.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-226">The first parameter of the **orderBy** method is a string equal to the name of the field on which to sort.</span></span> <span data-ttu-id="0d2e6-227">Druhý parametr používá **QueryOrder** výčtu k určení, jestli se má seřadit vzestupně nebo sestupně.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-227">The second parameter uses the **QueryOrder** enumeration to specify whether to sort ascending or descending.</span></span>  <span data-ttu-id="0d2e6-228">Filtrování pomocí ***kde*** metody ***kde*** metoda musí být volána před ***orderBy*** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-228">If you are filtering using the ***where*** method, the ***where*** method must be invoked before the ***orderBy*** method.</span></span>

### <span data-ttu-id="0d2e6-229"><a name="selection"></a>Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="0d2e6-229"><a name="selection"></a>Select specific columns</span></span>

<span data-ttu-id="0d2e6-230">Následující kód ukazuje, jak vrátit všechny položky z tabulky **ToDoItems**, ale zobrazuje jenom **dokončení** a **text** pole.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-230">The following code illustrates how to return all items from a table of **ToDoItems**, but only displays the **complete** and **text** fields.</span></span> <span data-ttu-id="0d2e6-231">**mToDoTable** se odkaz na tabulku back-end, která jsme vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-231">**mToDoTable** is the reference to the backend table that we created previously.</span></span>

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-232">Parametry vyberte funkce jsou řetězec názvy sloupců v tabulce, které chcete vrátit.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-232">The parameters to the select function are the string names of the table's columns that you want to return.</span></span>  <span data-ttu-id="0d2e6-233">**Vyberte** musí postupovat podle metody, třeba metoda **kde** a **orderBy**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-233">The **select** method needs to follow methods like **where** and **orderBy**.</span></span> <span data-ttu-id="0d2e6-234">Může následovat stránkování metody, třeba **přeskočit** a **horní**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-234">It can be followed by paging methods like **skip** and **top**.</span></span>

### <span data-ttu-id="0d2e6-235"><a name="paging"></a>Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="0d2e6-235"><a name="paging"></a>Return data in pages</span></span>

<span data-ttu-id="0d2e6-236">Data jsou **vždy** vrátil na stránkách.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-236">Data is **ALWAYS** returned in pages.</span></span>  <span data-ttu-id="0d2e6-237">Maximální počet záznamů vrácených nastavena serverem.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-237">The maximum number of records returned is set by the server.</span></span>  <span data-ttu-id="0d2e6-238">Pokud klient požaduje další záznamy, server vrátí maximální počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-238">If the client requests more records, then the server returns the maximum number of records.</span></span>  <span data-ttu-id="0d2e6-239">Ve výchozím nastavení je maximální velikost stránky na serveru 50 záznamů.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-239">By default, the maximum page size on the server is 50 records.</span></span>

<span data-ttu-id="0d2e6-240">V prvním příkladu ukazuje, jak vybrat prvních pět položek z tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-240">The first example shows how to select the top five items from a table.</span></span> <span data-ttu-id="0d2e6-241">Dotaz vrátí položky z tabulky **ToDoItems**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-241">The query returns the items from a table of **ToDoItems**.</span></span> <span data-ttu-id="0d2e6-242">**mToDoTable** se odkaz na tabulku back-end, který jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-242">**mToDoTable** is the reference to the backend table that you created previously:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-243">Zde je dotaz, který přeskočí první pěti položek a vrátí další pět:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-243">Here's a query that skips the first five items, and then returns the next five:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

<span data-ttu-id="0d2e6-244">Pokud chcete získat všechny záznamy v tabulce, implementaci kódu Iterujte přes všechny stránky:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-244">If you wish to get all records in a table, implement code to iterate over all pages:</span></span>

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

<span data-ttu-id="0d2e6-245">Žádost o pro všechny záznamy pomocí této metody vytvoří minimálně dva požadavky na back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-245">A request for all records using this method creates a minimum of two requests to the Mobile Apps backend.</span></span>

> [!TIP]
> <span data-ttu-id="0d2e6-246">Volba velikosti pravá stránka je rovnováhu mezi využití paměti při žádosti se děje, využití šířky pásma a zpoždění při přijímání dat úplně.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-246">Choosing the right page size is a balance between memory usage while the request is happening, bandwidth usage and delay in receiving the data completely.</span></span>  <span data-ttu-id="0d2e6-247">Výchozí hodnota (50 záznamy) je vhodná pro všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-247">The default (50 records) is suitable for all devices.</span></span>  <span data-ttu-id="0d2e6-248">Pokud provozujete výhradně na větší paměti zařízení, zvýšit až 500.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-248">If you exclusively operate on larger memory devices, increase up to 500.</span></span>  <span data-ttu-id="0d2e6-249">Našli jsme, zvýšení velikosti stránky nad rámec 500 zaznamenává výsledky v zpožděním a velké paměti problémy.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-249">We have found that increasing the page size beyond 500 records results in unacceptable delays and large memory issues.</span></span>

### <span data-ttu-id="0d2e6-250"><a name="chaining"></a>Postupy: řetězení metody dotazů</span><span class="sxs-lookup"><span data-stu-id="0d2e6-250"><a name="chaining"></a>How to: Concatenate query methods</span></span>

<span data-ttu-id="0d2e6-251">Může být zřetězen metody použité v dotazování tabulky back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-251">The methods used in querying backend tables can be concatenated.</span></span> <span data-ttu-id="0d2e6-252">Řetězení metody dotazů můžete vybrat konkrétní sloupce filtrované řádků, které jsou seřazené a stránkovaného fondu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-252">Chaining query methods allows you to select specific columns of filtered rows that are sorted and paged.</span></span> <span data-ttu-id="0d2e6-253">Můžete vytvořit komplexní logické filtry.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-253">You can create complex logical filters.</span></span>  <span data-ttu-id="0d2e6-254">Každá metoda dotaz vrátí objekt dotazu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-254">Each query method returns a Query object.</span></span> <span data-ttu-id="0d2e6-255">Pokud chcete ukončit řady metod a ve skutečnosti spusťte dotaz, volání **provést** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-255">To end the series of methods and actually run the query, call the **execute** method.</span></span> <span data-ttu-id="0d2e6-256">Například:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-256">For example:</span></span>

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

<span data-ttu-id="0d2e6-257">Zřetězené dotazu metody musejí být seřazeny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-257">The chained query methods must be ordered as follows:</span></span>

1. <span data-ttu-id="0d2e6-258">Filtrování (**kde**) metody.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-258">Filtering (**where**) methods.</span></span>
2. <span data-ttu-id="0d2e6-259">Řazení (**orderBy**) metody.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-259">Sorting (**orderBy**) methods.</span></span>
3. <span data-ttu-id="0d2e6-260">Výběr (**vyberte**) metody.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-260">Selection (**select**) methods.</span></span>
4. <span data-ttu-id="0d2e6-261">stránkování (**přeskočit** a **horní**) metody.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-261">paging (**skip** and **top**) methods.</span></span>

## <span data-ttu-id="0d2e6-262"><a name="binding"></a>Vytvoření vazby dat s uživatelským rozhraním</span><span class="sxs-lookup"><span data-stu-id="0d2e6-262"><a name="binding"></a>Bind data to the user interface</span></span>

<span data-ttu-id="0d2e6-263">Datová vazba zahrnuje tři komponenty:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-263">Data binding involves three components:</span></span>

* <span data-ttu-id="0d2e6-264">Zdroj dat</span><span class="sxs-lookup"><span data-stu-id="0d2e6-264">The data source</span></span>
* <span data-ttu-id="0d2e6-265">Na obrazovce rozložení</span><span class="sxs-lookup"><span data-stu-id="0d2e6-265">The screen layout</span></span>
* <span data-ttu-id="0d2e6-266">Adaptér, který sváže dva společně.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-266">The adapter that ties the two together.</span></span>

<span data-ttu-id="0d2e6-267">V našem ukázkový kód jsme vrátit data z tabulky Mobile Apps SQL Azure **ToDoItem** na pole.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-267">In our sample code, we return the data from the Mobile Apps SQL Azure table **ToDoItem** into an array.</span></span> <span data-ttu-id="0d2e6-268">Tato aktivita je běžný vzor pro data aplikací.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-268">This activity is a common pattern for data applications.</span></span>  <span data-ttu-id="0d2e6-269">Databázové dotazy často vracet kolekci řádků, které klient získá v seznamu nebo pole.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-269">Database queries often return a collection of rows that the client gets in a list or array.</span></span> <span data-ttu-id="0d2e6-270">V této ukázce je pole zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-270">In this sample, the array is the data source.</span></span>  <span data-ttu-id="0d2e6-271">Kód určuje rozložení obrazovky, který definuje zobrazení dat, která se zobrazí v zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-271">The code specifies a screen layout that defines the view of the data that appears on the device.</span></span>  <span data-ttu-id="0d2e6-272">Dva jsou svázané s společně s adaptér, který tento kód je rozšířením z **ArrayAdapter&lt;ToDoItem&gt;**  třídy.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-272">The two are bound together with an adapter, which in this code is an extension of the **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span>

#### <span data-ttu-id="0d2e6-273"><a name="layout"></a>Definování rozložení</span><span class="sxs-lookup"><span data-stu-id="0d2e6-273"><a name="layout"></a>Define the Layout</span></span>

<span data-ttu-id="0d2e6-274">Rozložení je definována několik fragmenty kódu XML.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-274">The layout is defined by several snippets of XML code.</span></span> <span data-ttu-id="0d2e6-275">Vzhledem existující rozložení, následující kód představuje **ListView** chceme naplnění našich dat serveru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-275">Given an existing layout, the following code represents the **ListView** we want to populate with our server data.</span></span>

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

<span data-ttu-id="0d2e6-276">V předchozí kód *listitem* atribut určuje id rozložení pro jednotlivých řádků v seznamu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-276">In the preceding code, the *listitem* attribute specifies the id of the layout for an individual row in the list.</span></span> <span data-ttu-id="0d2e6-277">Tento kód určuje zaškrtávací políčko a jeho přidružené textu a získá instanci jednou pro každou položku v seznamu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-277">This code specifies a check box and its associated text and gets instantiated once for each item in the list.</span></span> <span data-ttu-id="0d2e6-278">Toto rozložení nezobrazí **id** pole a složitější rozložení by zadejte další pole v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-278">This layout does not display the **id** field, and a more complex layout would specify additional fields in the display.</span></span> <span data-ttu-id="0d2e6-279">Tento kód je v **row_list_to_do.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-279">This code is in the **row_list_to_do.xml** file.</span></span>

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

#### <span data-ttu-id="0d2e6-280"><a name="adapter"></a>Zadejte adaptér</span><span class="sxs-lookup"><span data-stu-id="0d2e6-280"><a name="adapter"></a>Define the adapter</span></span>
<span data-ttu-id="0d2e6-281">Vzhledem k tomu, že je zdroj dat naše zobrazení pole **ToDoItem**, jsme podtřídami adaptéru v našem **ArrayAdapter&lt;ToDoItem&gt;**  třídy.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-281">Since the data source of our view is an array of **ToDoItem**, we subclass our adapter from an **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span> <span data-ttu-id="0d2e6-282">Tato podtřídami vytvoří zobrazení pro každý **ToDoItem** pomocí **row_list_to_do** rozložení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-282">This subclass produces a View for every **ToDoItem** using the **row_list_to_do** layout.</span></span>  <span data-ttu-id="0d2e6-283">V našem kódu jsme definovali následující třídy, která je rozšířením **ArrayAdapter&lt;E&gt;**  třídy:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-283">In our code, we define the following class that is an extension of the **ArrayAdapter&lt;E&gt;** class:</span></span>

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

<span data-ttu-id="0d2e6-284">Přepsání adaptéry **getView** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-284">Override the adapters **getView** method.</span></span> <span data-ttu-id="0d2e6-285">Například:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-285">For example:</span></span>

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

<span data-ttu-id="0d2e6-286">Vytvoříme instance této třídy v našem aktivity následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-286">We create an instance of this class in our Activity as follows:</span></span>

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

<span data-ttu-id="0d2e6-287">Druhý parametr konstruktoru ToDoItemAdapter je odkaz na rozložení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-287">The second parameter to the ToDoItemAdapter constructor is a reference to the layout.</span></span> <span data-ttu-id="0d2e6-288">Nyní jsme můžete vytvořit instanci **ListView** a přiřaďte adaptér, který má **ListView**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-288">We can now instantiate the **ListView** and assign the adapter to the **ListView**.</span></span>

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <span data-ttu-id="0d2e6-289"><a name="use-adapter"></a>Adaptér pro vazbu na uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="0d2e6-289"><a name="use-adapter"></a>Use the Adapter to Bind to the UI</span></span>

<span data-ttu-id="0d2e6-290">Nyní jste připraveni používat datové vazby.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-290">You are now ready to use data binding.</span></span> <span data-ttu-id="0d2e6-291">Následující kód ukazuje, jak získat položky v tabulce a výplní místní adaptéru s vrácené položky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-291">The following code shows how to get items in the table and fills the local adapter with the returned items.</span></span>

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

<span data-ttu-id="0d2e6-292">Volání adaptér vždy, když upravíte **ToDoItem** tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-292">Call the adapter any time you modify the **ToDoItem** table.</span></span> <span data-ttu-id="0d2e6-293">Vzhledem k tomu, že změny se provádí na základě záznamu podle, můžete zpracovat jeden řádek místo kolekce.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-293">Since modifications are done on a record by record basis, you handle a single row instead of a collection.</span></span> <span data-ttu-id="0d2e6-294">Když vložíte položku, volání **přidat** metoda na adaptéru; při odstraňování, volání **odebrat** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-294">When you insert an item, call the **add** method on the adapter; when deleting, call the **remove** method.</span></span>

<span data-ttu-id="0d2e6-295">Kompletní příklad v můžete najít [projekt Android rychlý Start][21].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-295">You can find a complete example in the [Android Quickstart Project][21].</span></span>

## <span data-ttu-id="0d2e6-296"><a name="inserting"></a>Vložení dat do back-end</span><span class="sxs-lookup"><span data-stu-id="0d2e6-296"><a name="inserting"></a>Insert data into the backend</span></span>

<span data-ttu-id="0d2e6-297">Vytvoří instanci *ToDoItem* a nastavit jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-297">Instantiate an instance of the *ToDoItem* class and set its properties.</span></span>

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

<span data-ttu-id="0d2e6-298">Potom pomocí **insert()** o vložení objektu:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-298">Then use **insert()** to insert an object:</span></span>

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="0d2e6-299">Odpovídá vrácenou entitu data vložená do tabulky back-end, obsahovala ID a všechny ostatní hodnoty (například `createdAt`, `updatedAt`, a `version` pole) nastavte na back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-299">The returned entity matches the data inserted into the backend table, included the ID and any other values (such as the `createdAt`, `updatedAt`, and `version` fields) set on the backend.</span></span>

<span data-ttu-id="0d2e6-300">Mobile Apps tabulky vyžadují sloupec primárního klíče s názvem **id**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-300">Mobile Apps tables require a primary key column named **id**.</span></span> <span data-ttu-id="0d2e6-301">Tento sloupec musí být řetězec.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-301">This column must be a string.</span></span> <span data-ttu-id="0d2e6-302">Výchozí hodnota ID sloupce je identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-302">The default value of the ID column is a GUID.</span></span>  <span data-ttu-id="0d2e6-303">Můžete zadat další jedinečné hodnoty, například e-mailové adresy nebo uživatelských jmen.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-303">You can provide other unique values, such as email addresses or usernames.</span></span> <span data-ttu-id="0d2e6-304">Pokud není zadána hodnota ID řetězec vloženého záznamu, back-end generuje nový identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-304">When a string ID value is not provided for an inserted record, the backend generates a new GUID.</span></span>

<span data-ttu-id="0d2e6-305">Hodnota ID řetězec poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-305">String ID values provide the following advantages:</span></span>

* <span data-ttu-id="0d2e6-306">ID může být generována bez provedení výměnu zpráv do databáze.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-306">IDs can be generated without making a round trip to the database.</span></span>
* <span data-ttu-id="0d2e6-307">Záznamy jsou usnadňují sloučení z různých tabulek nebo databází.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-307">Records are easier to merge from different tables or databases.</span></span>
* <span data-ttu-id="0d2e6-308">Hodnoty ID lépe integrovat logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-308">ID values integrate better with an application's logic.</span></span>

<span data-ttu-id="0d2e6-309">Řetězec ID hodnoty jsou **REQUIRED** pro podporu offline synchronizace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-309">String ID values are **REQUIRED** for offline sync support.</span></span>  <span data-ttu-id="0d2e6-310">Id nelze změnit, jakmile je uložen v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-310">You cannot change an Id once it is stored in the backend database.</span></span>

## <span data-ttu-id="0d2e6-311"><a name="updating"></a>Aktualizovat data v mobilní aplikaci</span><span class="sxs-lookup"><span data-stu-id="0d2e6-311"><a name="updating"></a>Update data in a mobile app</span></span>

<span data-ttu-id="0d2e6-312">Pokud chcete aktualizovat data v tabulce, předat nový objekt, který má **update()** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-312">To update data in a table, pass the new object to the **update()** method.</span></span>

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="0d2e6-313">V tomto příkladu *položky* je odkaz na řádek *ToDoItem* tabulku, která se použila některé změny.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-313">In this example, *item* is a reference to a row in the *ToDoItem* table, which has had some changes made to it.</span></span>  <span data-ttu-id="0d2e6-314">Řádek se stejným **id** se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-314">The row with the same **id** is updated.</span></span>

## <span data-ttu-id="0d2e6-315"><a name="deleting"></a>Odstranit data v mobilní aplikaci</span><span class="sxs-lookup"><span data-stu-id="0d2e6-315"><a name="deleting"></a>Delete data in a mobile app</span></span>

<span data-ttu-id="0d2e6-316">Následující kód ukazuje, jak k odstranění dat z tabulky zadáním datový objekt.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-316">The following code shows how to delete data from a table by specifying the data object.</span></span>

```java
mToDoTable
    .delete(item);
```

<span data-ttu-id="0d2e6-317">Můžete také odstranit položku zadáním **id** pole řádku odstranit.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-317">You can also delete an item by specifying the **id** field of the row to delete.</span></span>

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <span data-ttu-id="0d2e6-318"><a name="lookup"></a>Vyhledat konkrétní položky podle Id</span><span class="sxs-lookup"><span data-stu-id="0d2e6-318"><a name="lookup"></a>Look up a specific item by Id</span></span>

<span data-ttu-id="0d2e6-319">Vyhledání položky s konkrétní **id** pole s **lookUp()** metoda:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-319">Look up an item with a specific **id** field with the **lookUp()** method:</span></span>

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <span data-ttu-id="0d2e6-320"><a name="untyped"></a>Postupy: práce s daty bez typu</span><span class="sxs-lookup"><span data-stu-id="0d2e6-320"><a name="untyped"></a>How to: Work with untyped data</span></span>

<span data-ttu-id="0d2e6-321">Netypové programovací model poskytuje přesnou kontrolu nad serializace JSON.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-321">The untyped programming model gives you exact control over JSON serialization.</span></span>  <span data-ttu-id="0d2e6-322">Existují některé běžné scénáře, kde můžete chtít použít bez typu programovací model.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-322">There are some common scenarios where you may wish to use an untyped programming model.</span></span> <span data-ttu-id="0d2e6-323">Pokud například vaše back-end tabulka obsahuje mnoho sloupců a potřebujete odkazovat na podmnožinu sloupců.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-323">For example, if your backend table contains many columns and you only need to reference a subset of the columns.</span></span>  <span data-ttu-id="0d2e6-324">Typové modelu vyžaduje definování všechny sloupce definované v back-end mobilní aplikace v třídě data.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-324">The typed model requires you to define all the columns defined in the Mobile Apps backend in your data class.</span></span>  <span data-ttu-id="0d2e6-325">Většina volání rozhraní API pro přístup k datům je podobný typu programovací volání.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-325">Most of the API calls for accessing data are similar to the typed programming calls.</span></span> <span data-ttu-id="0d2e6-326">Hlavní rozdíl je, že v netypové modelu můžete volat metody na **MobileServiceJsonTable** objekt, místo **MobileServiceTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-326">The main difference is that in the untyped model you invoke methods on the **MobileServiceJsonTable** object, instead of the **MobileServiceTable** object.</span></span>

### <span data-ttu-id="0d2e6-327"><a name="json_instance"></a>Vytvoření instance bez typu tabulky</span><span class="sxs-lookup"><span data-stu-id="0d2e6-327"><a name="json_instance"></a>Create an instance of an untyped table</span></span>

<span data-ttu-id="0d2e6-328">Podobně jako u typu modelu, můžete začít nastavením odkaz na tabulku, ale v takovém případě je **MobileServicesJsonTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-328">Similar to the typed model, you start by getting a table reference, but in this case it's a **MobileServicesJsonTable** object.</span></span> <span data-ttu-id="0d2e6-329">Získat odkaz na voláním **jít** metoda na instanci klienta:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-329">Obtain the reference by calling the **getTable** method on an instance of the client:</span></span>

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

<span data-ttu-id="0d2e6-330">Po vytvoření instance **MobileServiceJsonTable**, má skoro stejný API, které jsou k dispozici jako s typem programovací model.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-330">Once you have created an instance of the **MobileServiceJsonTable**, it has virtually the same API available as with the typed programming model.</span></span> <span data-ttu-id="0d2e6-331">V některých případech metody trvat bez typu parametru místo typu parametru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-331">In some cases, the methods take an untyped parameter instead of a typed parameter.</span></span>

### <span data-ttu-id="0d2e6-332"><a name="json_insert"></a>Vložit do tabulky bez typu</span><span class="sxs-lookup"><span data-stu-id="0d2e6-332"><a name="json_insert"></a>Insert into an untyped table</span></span>
<span data-ttu-id="0d2e6-333">Následující kód ukazuje, jak udělat typu vložení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-333">The following code shows how to do an insert.</span></span> <span data-ttu-id="0d2e6-334">Prvním krokem je vytvoření [JsonObject][1], který je součástí [gson] [ 3] knihovny.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-334">The first step is to create a [JsonObject][1], which is part of the [gson][3] library.</span></span>

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

<span data-ttu-id="0d2e6-335">Poté použijte **insert()** netypové objekt vložit do tabulky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-335">Then, Use **insert()** to insert the untyped object into the table.</span></span>

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

<span data-ttu-id="0d2e6-336">Pokud potřebujete získat ID vložené objektu, použijte **getAsJsonPrimitive()** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-336">If you need to get the ID of the inserted object, use the **getAsJsonPrimitive()** method.</span></span>

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <span data-ttu-id="0d2e6-337"><a name="json_delete"></a>Odstraňte z bez typu tabulky</span><span class="sxs-lookup"><span data-stu-id="0d2e6-337"><a name="json_delete"></a>Delete from an untyped table</span></span>
<span data-ttu-id="0d2e6-338">Následující kód ukazuje, jak odstranit instance, v takovém případě stejnou instanci **JsonObject** který byl vytvořen v předchozího *vložit* příklad.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-338">The following code shows how to delete an instance, in this case, the same instance of a **JsonObject** that was created in the prior *insert* example.</span></span> <span data-ttu-id="0d2e6-339">Kód je stejný jako s typem případ, ale metoda má jiný podpis, protože odkazuje na **JsonObject**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-339">The code is the same as with the typed case, but the method has a different signature since it references an **JsonObject**.</span></span>

```java
mToDoTable
    .delete(insertedItem);
```

<span data-ttu-id="0d2e6-340">Můžete také odstranit instanci přímo pomocí jeho ID:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-340">You can also delete an instance directly by using its ID:</span></span>

```java
mToDoTable.delete(ID);
```

### <span data-ttu-id="0d2e6-341"><a name="json_get"></a>Vrátí všechny řádky z tabulky aplikace bez typu</span><span class="sxs-lookup"><span data-stu-id="0d2e6-341"><a name="json_get"></a>Return all rows from an untyped table</span></span>
<span data-ttu-id="0d2e6-342">Následující kód ukazuje, jak načíst celou tabulku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-342">The following code shows how to retrieve an entire table.</span></span> <span data-ttu-id="0d2e6-343">Vzhledem k tomu, že používáte JSON tabulky, můžete selektivně načíst jenom některé sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-343">Since you are using a JSON Table, you can selectively retrieve only some of the table's columns.</span></span>

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

<span data-ttu-id="0d2e6-344">Stejnou sadu filtrování, filtrování a stránkování metody, které jsou k dispozici pro typové modelu jsou k dispozici bez typu modelu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-344">The same set of filtering, filtering and paging methods that are available for the typed model are available for the untyped model.</span></span>

## <span data-ttu-id="0d2e6-345"><a name="offline-sync"></a>Implementace Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-345"><a name="offline-sync"></a>Implement Offline Sync</span></span>

<span data-ttu-id="0d2e6-346">Azure Mobile Apps Client SDK také implementuje offline synchronizace dat s použitím databáze SQLite k uložení kopie dat serveru místně.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-346">The Azure Mobile Apps Client SDK also implements offline synchronization of data by using a SQLite database to store a copy of the server data locally.</span></span>  <span data-ttu-id="0d2e6-347">Operace provedené v offline tabulce nevyžadují mobilní připojení fungovat.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-347">Operations performed on an offline table do not require mobile connectivity to work.</span></span>  <span data-ttu-id="0d2e6-348">Offline synchronizace je výhodné při odolnost a výkon za cenu složitější logiku pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-348">Offline sync aids in resilience and performance at the expense of more complex logic for conflict resolution.</span></span>  <span data-ttu-id="0d2e6-349">Azure Mobile Apps Client SDK implementuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-349">The Azure Mobile Apps Client SDK implements the following features:</span></span>

* <span data-ttu-id="0d2e6-350">Přírůstkové synchronizace: Pouze aktualizované a nové záznamy se stáhnou, ukládání spotřebu šířky pásma a paměti.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-350">Incremental Sync: Only updated and new records are downloaded, saving bandwidth and memory consumption.</span></span>
* <span data-ttu-id="0d2e6-351">Optimistickou metodu souběžného: Operace se předpokládá, že proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-351">Optimistic Concurrency: Operations are assumed to succeed.</span></span>  <span data-ttu-id="0d2e6-352">Řešení konfliktů je odložení, dokud nebude aktualizace se provádí na serveru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-352">Conflict Resolution is deferred until updates are performed on the server.</span></span>
* <span data-ttu-id="0d2e6-353">Řešení konfliktů: Sada SDK zjistí, že změna způsobující konflikt byl změněn na serveru a poskytuje háky k upozornění uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-353">Conflict Resolution: The SDK detects when a conflicting change has been made at the server and provides hooks to alert the user.</span></span>
* <span data-ttu-id="0d2e6-354">Obnovitelného odstranění: Odstraněné záznamy jsou označené odstraněné, povolení jiná zařízení k aktualizaci mezipaměti v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-354">Soft Delete: Deleted records are marked deleted, allowing other devices to update their offline cache.</span></span>

### <a name="initialize-offline-sync"></a><span data-ttu-id="0d2e6-355">Inicializaci Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-355">Initialize Offline Sync</span></span>

<span data-ttu-id="0d2e6-356">Každá tabulka offline musí být definován v mezipaměti offline před použitím.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-356">Each offline table must be defined in the offline cache before use.</span></span>  <span data-ttu-id="0d2e6-357">Za normálních okolností se okamžitě po vytvoření klienta provádí definici tabulky:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-357">Normally, table definition is done immediately after the creation of the client:</span></span>

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

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
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

### <a name="obtain-a-reference-to-the-offline-cache-table"></a><span data-ttu-id="0d2e6-358">Získat odkaz na tabulky mezipaměti v režimu Offline</span><span class="sxs-lookup"><span data-stu-id="0d2e6-358">Obtain a reference to the Offline Cache Table</span></span>

<span data-ttu-id="0d2e6-359">Pro online tabulky, můžete použít `.getTable()`.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-359">For an online table, you use `.getTable()`.</span></span>  <span data-ttu-id="0d2e6-360">K offline tabulky, použijte `.getSyncTable()`:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-360">For an offline table, use `.getSyncTable()`:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

<span data-ttu-id="0d2e6-361">Všechny metody, které jsou k dispozici pro online tabulky (včetně filtrování, řazení, stránkování, vkládání dat, aktualizace dat a odstraňování dat) pracovat stejně dobře u tabulek se online a offline.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-361">All the methods that are available for online tables (including filtering, sorting, paging, inserting data, updating data, and deleting data) work equally well on online and offline tables.</span></span>

### <a name="synchronize-the-local-offline-cache"></a><span data-ttu-id="0d2e6-362">Synchronizovat místní mezipaměti v režimu Offline</span><span class="sxs-lookup"><span data-stu-id="0d2e6-362">Synchronize the Local Offline Cache</span></span>

<span data-ttu-id="0d2e6-363">Synchronizace je v rámci prvku aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-363">Synchronization is within the control of your app.</span></span>  <span data-ttu-id="0d2e6-364">Tady je příklad metoda synchronizace:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-364">Here is an example synchronization method:</span></span>

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

<span data-ttu-id="0d2e6-365">Pokud je pro zadaný název dotazu `.pull(query, queryname)` metoda pak přírůstkové synchronizace se používá k vrácení pouze záznamy, které byly vytvořeny nebo změněné od posledního úspěšně dokončit vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-365">If a query name is provided to the `.pull(query, queryname)` method, then incremental sync is used to return only records that have been created or changed since the last successfully completed pull.</span></span>

### <a name="handle-conflicts-during-offline-synchronization"></a><span data-ttu-id="0d2e6-366">Zpracování konfliktů během Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-366">Handle Conflicts during Offline Synchronization</span></span>

<span data-ttu-id="0d2e6-367">Pokud dojde ke konfliktu při `.push()` operace, `MobileServiceConflictException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-367">If a conflict happens during a `.push()` operation, a `MobileServiceConflictException` is thrown.</span></span>   <span data-ttu-id="0d2e6-368">Položka server vydal vložené do výjimku, může načíst `.getItem()` na výjimku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-368">The server-issued item is embedded in the exception and can be retrieved by `.getItem()` on the exception.</span></span>  <span data-ttu-id="0d2e6-369">Upravte nabízeného oznámení při volání objektu MobileServiceSyncContext následující položky:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-369">Adjust the push by calling the following items on the MobileServiceSyncContext object:</span></span>

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

<span data-ttu-id="0d2e6-370">Jakmile se všechny konflikty jsou označené jako nechcete, volání `.push()` znovu a vyřešte všechny konflikty.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-370">Once all conflicts are marked as you wish, call `.push()` again to resolve all the conflicts.</span></span>

## <span data-ttu-id="0d2e6-371"><a name="custom-api"></a>Volání vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0d2e6-371"><a name="custom-api"></a>Call a custom API</span></span>

<span data-ttu-id="0d2e6-372">Vlastní rozhraní API umožňuje definovat vlastní koncové body, které zveřejňují funkce serveru které není mapovat k typu vložení, aktualizaci, odstranění nebo operace čtení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-372">A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation.</span></span> <span data-ttu-id="0d2e6-373">Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-373">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="0d2e6-374">V klientovi aplikace Android zavoláte **invokeApi** metoda k volání vlastní koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-374">From an Android client, you call the **invokeApi** method to call the custom API endpoint.</span></span> <span data-ttu-id="0d2e6-375">Následující příklad ukazuje způsob volání rozhraní API koncový bod s názvem **completeAll**, který vrátí kolekce třídy s názvem **MarkAllResult**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-375">The following example shows how to call an API endpoint named **completeAll**, which returns a collection class named **MarkAllResult**.</span></span>

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

<span data-ttu-id="0d2e6-376">**InvokeApi** metoda je volána v klientovi, který odešle požadavek POST do nové vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-376">The **invokeApi** method is called on the client, which sends a POST request to the new custom API.</span></span> <span data-ttu-id="0d2e6-377">Výsledek vrácený vlastního rozhraní API zobrazí v dialogu zprávy, jako jsou všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-377">The result returned by the custom API is displayed in a message dialog, as are any errors.</span></span> <span data-ttu-id="0d2e6-378">Jiné verze **invokeApi** umožňují volitelně odesílat objekt v textu požadavku, zadejte metodu protokolu HTTP a odeslat parametry dotazu s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-378">Other versions of **invokeApi** let you optionally send an object in the request body, specify the HTTP method, and send query parameters with the request.</span></span> <span data-ttu-id="0d2e6-379">Netypová verze **invokeApi** k dispozici jsou také.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-379">Untyped versions of **invokeApi** are provided as well.</span></span>

## <span data-ttu-id="0d2e6-380"><a name="authentication"></a>Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-380"><a name="authentication"></a>Add authentication to your app</span></span>

<span data-ttu-id="0d2e6-381">Podrobné kurzy již popisují postup přidání těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-381">Tutorials already describe in detail how to add these features.</span></span>

<span data-ttu-id="0d2e6-382">App Service podporuje [ověřování uživatelů aplikace](app-service-mobile-android-get-started-users.md) pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-382">App Service supports [authenticating app users](app-service-mobile-android-get-started-users.md) using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="0d2e6-383">Můžete nastavit oprávnění pro tabulky, pokud chcete omezit přístup pro určité operace pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-383">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="0d2e6-384">Můžete také použít identitu ověřeného uživatele k implementaci autorizační pravidla v váš back-end.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-384">You can also use the identity of authenticated users to implement authorization rules in your backend.</span></span>

<span data-ttu-id="0d2e6-385">Jsou podporovány dva ověřování toky: **server** toku a **klienta** toku.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-385">Two authentication flows are supported: a **server** flow and a **client** flow.</span></span> <span data-ttu-id="0d2e6-386">Tok serveru poskytuje nejjednodušší zkušeností ověřování, jako je závislé na webové rozhraní pro zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-386">The server flow provides the simplest authentication experience, as it relies on the identity providers web interface.</span></span>  <span data-ttu-id="0d2e6-387">Žádné další sady SDK jsou nutné k implementaci tok ověřování serveru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-387">No additional SDKs are required to implement server flow authentication.</span></span> <span data-ttu-id="0d2e6-388">Tok ověřování serveru neposkytuje těsná integrace do mobilního zařízení a doporučuje se pouze pro testování konceptu scénáře.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-388">Server flow authentication does not provide a deep integration into the mobile device and is only recommended for proof of concept scenarios.</span></span>

<span data-ttu-id="0d2e6-389">Tok klienta umožňuje hlubší integrace s funkcí konkrétní zařízení, jako je jednotné přihlašování jako přitom spoléhá na sady SDK od zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-389">The client flow allows for deeper integration with device-specific capabilities such as single sign-on as it relies on SDKs provided by the identity provider.</span></span>  <span data-ttu-id="0d2e6-390">Například můžete integrovat Facebook SDK do své mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-390">For example, you can integrate the Facebook SDK into your mobile application.</span></span>  <span data-ttu-id="0d2e6-391">Mobilního klienta umožňuje přepnout do aplikace Facebook a potvrdí, vaše přihlášení před odkládací zpět do mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-391">The mobile client swaps into the Facebook app and confirms your sign-on before swapping back to your mobile app.</span></span>

<span data-ttu-id="0d2e6-392">Čtyři kroky jsou nezbytné pro povolení ověřování v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-392">Four steps are required to enable authentication in your app:</span></span>

* <span data-ttu-id="0d2e6-393">Registrace aplikace pro ověřování pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-393">Register your app for authentication with an identity provider.</span></span>
* <span data-ttu-id="0d2e6-394">Konfigurace back-end vaší služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-394">Configure your App Service backend.</span></span>
* <span data-ttu-id="0d2e6-395">Omezte oprávnění tabulka ověřeného uživatele pouze na back-end služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-395">Restrict table permissions to authenticated users only on the App Service backend.</span></span>
* <span data-ttu-id="0d2e6-396">Přidání ověřovacího kódu do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-396">Add authentication code to your app.</span></span>

<span data-ttu-id="0d2e6-397">Můžete nastavit oprávnění pro tabulky, pokud chcete omezit přístup pro určité operace pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-397">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="0d2e6-398">Identifikátor SID ověřeného uživatele můžete také upravit požadavky.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-398">You can also use the SID of an authenticated user to modify requests.</span></span>  <span data-ttu-id="0d2e6-399">Další informace najdete v tématu [Začínáme s ověřováním] a v dokumentaci k serveru SDK postupy.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-399">For more information, review [Get started with authentication] and the Server SDK HOWTO documentation.</span></span>

### <span data-ttu-id="0d2e6-400"><a name="caching"></a>Ověřování: Tok serveru</span><span class="sxs-lookup"><span data-stu-id="0d2e6-400"><a name="caching"></a>Authentication: Server Flow</span></span>

<span data-ttu-id="0d2e6-401">Následující kód spustí proces serveru toku přihlášení pomocí zprostředkovatele Google.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-401">The following code starts a server flow login process using the Google provider.</span></span>  <span data-ttu-id="0d2e6-402">Z důvodu požadavků na zabezpečení u zprostředkovatele Google není nutná další konfigurace:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-402">Additional configuration is required because of the security requirements for the Google provider:</span></span>

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

<span data-ttu-id="0d2e6-403">Kromě toho přidejte následující metodu do hlavní třídy aktivity:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-403">In addition, add the following method to the main Activity class:</span></span>

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

<span data-ttu-id="0d2e6-404">`GOOGLE_LOGIN_REQUEST_CODE` Definované v vaší hlavní aktivita se používá pro `login()` metoda a v `onActivityResult()` metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-404">The `GOOGLE_LOGIN_REQUEST_CODE` defined in your main Activity is used for the `login()` method and within the `onActivityResult()` method.</span></span>  <span data-ttu-id="0d2e6-405">Jedinečné číslo, můžete tak dlouho, dokud se v rámci používá stejné číslo `login()` metoda a `onActivityResult()` metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-405">You can choose any unique number, as long as the same number is used within the `login()` method and the `onActivityResult()` method.</span></span>  <span data-ttu-id="0d2e6-406">Pokud jste abstraktní kód klienta do služby adaptér (jak je uvedeno výše), by měly volat metody odpovídající na adaptéru služby.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-406">If you abstract the client code into a service adapter (as shown earlier), you should call the appropriate methods on the service adapter.</span></span>

<span data-ttu-id="0d2e6-407">Musíte také nakonfigurovat projekt pro customtabs.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-407">You also need to configure the project for customtabs.</span></span>  <span data-ttu-id="0d2e6-408">Nejdřív zadejte adresu URL přesměrování.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-408">First specify a redirect-URL.</span></span>  <span data-ttu-id="0d2e6-409">Přidejte následující fragment k `AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-409">Add the following snippet to `AndroidManifest.xml`:</span></span>

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

<span data-ttu-id="0d2e6-410">Přidat **redirectUriScheme** k `build.gradle` souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-410">Add the **redirectUriScheme** to the `build.gradle` file for your application:</span></span>

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

<span data-ttu-id="0d2e6-411">Nakonec přidejte `com.android.support:customtabs:23.0.1` v seznamu závislostí `build.gradle` souboru:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-411">Finally, add `com.android.support:customtabs:23.0.1` to the dependencies list in the `build.gradle` file:</span></span>

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

<span data-ttu-id="0d2e6-412">Získání ID přihlášeného uživatele z **MobileServiceUser** pomocí **reprezentuje getUserId** metoda.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-412">Obtain the ID of the logged-in user from a **MobileServiceUser** using the **getUserId** method.</span></span> <span data-ttu-id="0d2e6-413">Příklad použití tříd Future k volání asynchronní přihlášení rozhraní API, naleznete v části [Začínáme s ověřováním].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-413">For an example of how to use Futures to call the asynchronous login APIs, see [Get started with authentication].</span></span>

> [!WARNING]
> <span data-ttu-id="0d2e6-414">Schéma adresy URL uvedené rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-414">The URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="0d2e6-415">Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-415">Ensure that all occurrences of `{url_scheme_of_you_app}` match case.</span></span>

### <span data-ttu-id="0d2e6-416"><a name="caching"></a>Tokeny ověřování do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="0d2e6-416"><a name="caching"></a>Cache authentication tokens</span></span>

<span data-ttu-id="0d2e6-417">Ukládání do mezipaměti tokeny ověřování vyžaduje, abyste pro ukládání ID uživatele a ověřovací token místně na zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-417">Caching authentication tokens requires you to store the User ID and authentication token locally on the device.</span></span> <span data-ttu-id="0d2e6-418">Při příštím spuštění aplikace, zkontrolujte mezipaměti, a pokud nejsou tyto hodnoty, můžete přeskočit protokolu v postupu a rehydrataci při spotřebě klienta se tato data.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-418">The next time the app starts, you check the cache, and if these values are present, you can skip the log in procedure and rehydrate the client with this data.</span></span> <span data-ttu-id="0d2e6-419">Ale tato data jsou citlivé a by měly být uložené šifrována pro zabezpečení v případě, že získá odcizení telefonu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-419">However this data is sensitive, and it should be stored encrypted for safety in case the phone gets stolen.</span></span>  <span data-ttu-id="0d2e6-420">Zobrazí úplný příklad toho, jak do mezipaměti ověřování tokenů v [mezipaměti část tokeny ověřování][7].</span><span class="sxs-lookup"><span data-stu-id="0d2e6-420">You can see a complete example of how to cache authentication tokens in [Cache authentication tokens section][7].</span></span>

<span data-ttu-id="0d2e6-421">Když se pokusíte použít tokenu vypršela platnost, zobrazí se *401 Neautorizováno* odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-421">When you try to use an expired token, you receive a *401 unauthorized* response.</span></span> <span data-ttu-id="0d2e6-422">Může zpracovávat ověřování chyb pomocí filtrů.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-422">You can handle authentication errors using filters.</span></span>  <span data-ttu-id="0d2e6-423">Filtry zachycení požadavků na back-end služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-423">Filters intercept requests to the App Service backend.</span></span> <span data-ttu-id="0d2e6-424">Kód filtru testy odpovědi na 401, spustí proces přihlášení a potom obnoví žádost, která generovala kód 401.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-424">The filter code tests the response for a 401, triggers the sign-in process, and then resumes the request that generated the 401.</span></span>

### <span data-ttu-id="0d2e6-425"><a name="refresh"></a>Použití obnovovacích tokenů</span><span class="sxs-lookup"><span data-stu-id="0d2e6-425"><a name="refresh"></a>Use Refresh Tokens</span></span>

<span data-ttu-id="0d2e6-426">Token vrácený Azure App Service ověřování a autorizace má definovaná životnosti jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-426">The token returned by Azure App Service Authentication and Authorization has a defined life time of one hour.</span></span>  <span data-ttu-id="0d2e6-427">Po uplynutí této doby musí novému ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-427">After this period, you must reauthenticate the user.</span></span>  <span data-ttu-id="0d2e6-428">Pokud používáte dlohotrvající token, který jste obdrželi prostřednictvím ověřování tok klienta a pak můžete novému ověření pomocí Azure App Service ověřování a autorizace pomocí jednoho tokenu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-428">If you are using a long-lived token that you have received via client-flow authentication, then you can reauthenticate with Azure App Service Authentication and Authorization using the same token.</span></span>  <span data-ttu-id="0d2e6-429">Další token služby Azure App Service je vytvořen s novou životnost.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-429">Another Azure App Service token is generated with a new lifetime.</span></span>

<span data-ttu-id="0d2e6-430">Můžete také registrovat zprostředkovatele, který má použít aktualizaci tokeny.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-430">You can also register the provider to use Refresh Tokens.</span></span>  <span data-ttu-id="0d2e6-431">Aktualizovat Token není vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-431">A Refresh Token is not always available.</span></span>  <span data-ttu-id="0d2e6-432">Je vyžadována další konfigurace:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-432">Additional configuration is required:</span></span>

* <span data-ttu-id="0d2e6-433">Pro **Azure Active Directory**, nakonfigurovat sdílený tajný klíč klienta pro aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-433">For **Azure Active Directory**, configure a client secret for the Azure Active Directory App.</span></span>  <span data-ttu-id="0d2e6-434">Zadejte sdílený tajný klíč klienta v Azure App Service při konfiguraci ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-434">Specify the client secret in the Azure App Service when configuring Azure Active Directory Authentication.</span></span>  <span data-ttu-id="0d2e6-435">Při volání metody `.login()`, předat `response_type=code id_token` jako parametr:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-435">When calling `.login()`, pass `response_type=code id_token` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="0d2e6-436">Pro **Google**, předat `access_type=offline` jako parametr:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-436">For **Google**, pass the `access_type=offline` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="0d2e6-437">Pro **Account Microsoft**, vyberte `wl.offline_access` oboru.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-437">For **Microsoft Account**, select the `wl.offline_access` scope.</span></span>

<span data-ttu-id="0d2e6-438">Chcete-li aktualizovat token, volejte `.refreshUser()`:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-438">To refresh a token, call `.refreshUser()`:</span></span>

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

<span data-ttu-id="0d2e6-439">Jako osvědčený postup vytvoření filtru, který zjistí 401 odpověď ze serveru a pokusí se aktualizovat token uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-439">As a best practice, create a filter that detects a 401 response from the server and tries to refresh the user token.</span></span>

## <a name="log-in-with-client-flow-authentication"></a><span data-ttu-id="0d2e6-440">Přihlaste se pomocí ověřování tok klienta</span><span class="sxs-lookup"><span data-stu-id="0d2e6-440">Log in with Client-flow Authentication</span></span>

<span data-ttu-id="0d2e6-441">Obecný postup přihlášení pomocí ověřování tok klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-441">The general process for logging in with client-flow authentication is as follows:</span></span>

* <span data-ttu-id="0d2e6-442">Konfigurace Azure App Service ověřování a autorizaci, stejně jako server tok ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-442">Configure Azure App Service Authentication and Authorization as you would server-flow authentication.</span></span>
* <span data-ttu-id="0d2e6-443">Integrate zprostředkovatele ověřování SDK pro ověřování a vytvořit token přístupu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-443">Integrate the authentication provider SDK for authentication to produce an access token.</span></span>
* <span data-ttu-id="0d2e6-444">Volání `.login()` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-444">Call the `.login()` method as follows:</span></span>

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

<span data-ttu-id="0d2e6-445">Nahraďte `onSuccess()` metoda s ať kódu je chcete použít v úspěšném přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-445">Replace the `onSuccess()` method with whatever code you wish to use on a successful login.</span></span>  <span data-ttu-id="0d2e6-446">`{provider}` Řetězec je platný zprostředkovatel: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, nebo **twitter**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-446">The `{provider}` string is a valid provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, or **twitter**.</span></span>  <span data-ttu-id="0d2e6-447">Pokud jste implementovali vlastní ověřování, můžete také použít poskytovatele značky vlastního ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-447">If you have implemented custom authentication, then you can also use the custom authentication provider tag.</span></span>

### <span data-ttu-id="0d2e6-448"><a name="adal"></a>Ověřování uživatelů pomocí Active Directory Authentication Library (ADAL)</span><span class="sxs-lookup"><span data-stu-id="0d2e6-448"><a name="adal"></a>Authenticate users with the Active Directory Authentication Library (ADAL)</span></span>

<span data-ttu-id="0d2e6-449">Active Directory Authentication Library (ADAL) můžete použít pro přihlášení uživatelů do vaší aplikace pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-449">You can use the Active Directory Authentication Library (ADAL) to sign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="0d2e6-450">Pomocí přihlášení toku klienta je často vhodnější než použít `loginAsync()` metody jak poskytuje více nativní UX chování a umožňuje pro další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-450">Using a client flow login is often preferable to using the `loginAsync()` methods as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="0d2e6-451">Podle konfigurace váš back-end mobilní aplikace při přihlášení AAD [jak nakonfigurovat App Service pro přihlášení služby Active Directory] [ 22] kurzu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-451">Configure your mobile app backend for AAD sign-in by following the [How to configure App Service for Active Directory login][22] tutorial.</span></span> <span data-ttu-id="0d2e6-452">Ujistěte se, že dokončení volitelný krok registrace nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-452">Make sure to complete the optional step of registering a native client application.</span></span>
2. <span data-ttu-id="0d2e6-453">Nainstalujte ADAL úpravou souboru build.gradle zahrnout následující definice:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-453">Install ADAL by modifying your build.gradle file to include the following definitions:</span></span>

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

1. <span data-ttu-id="0d2e6-454">Přidejte následující kód k vaší aplikaci, provedení náhrady následující:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-454">Add the following code to your application, making the following replacements:</span></span>

* <span data-ttu-id="0d2e6-455">Nahraďte **INSERT. AUTORITY zde** s názvem klienta, ve kterém jste zřídili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-455">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="0d2e6-456">Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-456">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span>
* <span data-ttu-id="0d2e6-457">Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta pro váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-457">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="0d2e6-458">Můžete získat ID klienta z **Upřesnit** v části **nastavení Azure Active Directory** na portálu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-458">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
* <span data-ttu-id="0d2e6-459">Nahraďte **INSERT klienta ID zde** s ID klienta, který jste zkopírovali z nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-459">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
* <span data-ttu-id="0d2e6-460">Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-460">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="0d2e6-461">Tato hodnota by měla být podobná *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-461">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

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

## <span data-ttu-id="0d2e6-462"><a name="filters"></a>Upravit komunikaci klienta se serverem</span><span class="sxs-lookup"><span data-stu-id="0d2e6-462"><a name="filters"></a>Adjust the Client-Server Communication</span></span>

<span data-ttu-id="0d2e6-463">Připojení klienta je obvykle základní připojení HTTP pomocí základní knihovny HTTP součástí sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-463">The Client connection is normally a basic HTTP connection using the underlying HTTP library supplied with the Android SDK.</span></span>  <span data-ttu-id="0d2e6-464">Tady je několik důvodů, proč byste měli změnit, který:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-464">There are several reasons why you would want to change that:</span></span>

* <span data-ttu-id="0d2e6-465">Chcete použít alternativní knihovny HTTP upravit vypršení časových limitů.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-465">You wish to use an alternate HTTP library to adjust timeouts.</span></span>
* <span data-ttu-id="0d2e6-466">Chcete poskytovat indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-466">You wish to provide a progress bar.</span></span>
* <span data-ttu-id="0d2e6-467">Chcete přidat vlastní hlavičku pro podporu funkcí správy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-467">You wish to add a custom header to support API management functionality.</span></span>
* <span data-ttu-id="0d2e6-468">Chcete zachytit neúspěšných odpovědí proto, že můžete implementovat opětovné ověření.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-468">You wish to intercept a failed response so that you can implement reauthentication.</span></span>
* <span data-ttu-id="0d2e6-469">Chcete protokolovat požadavky na back-end do služby analýzy.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-469">You wish to log backend requests to an analytics service.</span></span>

### <a name="using-an-alternate-http-library"></a><span data-ttu-id="0d2e6-470">Pomocí alternativní knihovny HTTP</span><span class="sxs-lookup"><span data-stu-id="0d2e6-470">Using an alternate HTTP Library</span></span>

<span data-ttu-id="0d2e6-471">Volání `.setAndroidHttpClientFactory()` metoda ihned po vytvoření odkaz na klienta.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-471">Call the `.setAndroidHttpClientFactory()` method immediately after creating your client reference.</span></span>  <span data-ttu-id="0d2e6-472">Chcete-li například nastavit časový limit připojení na 60 sekund (místo výchozího 10 sekund):</span><span class="sxs-lookup"><span data-stu-id="0d2e6-472">For example, to set the connection timeout to 60 seconds (instead of the default 10 seconds):</span></span>

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

### <a name="implement-a-progress-filter"></a><span data-ttu-id="0d2e6-473">Implementace filtru průběh</span><span class="sxs-lookup"><span data-stu-id="0d2e6-473">Implement a Progress Filter</span></span>

<span data-ttu-id="0d2e6-474">Zachycení každou žádost můžete implementovat implementací `ServiceFilter`.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-474">You can implement an intercept of every request by implementing a `ServiceFilter`.</span></span>  <span data-ttu-id="0d2e6-475">Následující aktualizace například předem vytvořené indikátor průběhu:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-475">For example, the following updates a pre-created progress bar:</span></span>

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

<span data-ttu-id="0d2e6-476">Tento filtr je možné připojit klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-476">You can attach this filter to the client as follows:</span></span>

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a><span data-ttu-id="0d2e6-477">Přizpůsobení hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="0d2e6-477">Customize Request Headers</span></span>

<span data-ttu-id="0d2e6-478">Použijte následující `ServiceFilter` a připojte filtr stejným způsobem jako `ProgressFilter`:</span><span class="sxs-lookup"><span data-stu-id="0d2e6-478">Use the following `ServiceFilter` and attach the filter in the same way as the `ProgressFilter`:</span></span>

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

### <span data-ttu-id="0d2e6-479"><a name="conversions"></a>Konfigurace automatického serializace</span><span class="sxs-lookup"><span data-stu-id="0d2e6-479"><a name="conversions"></a>Configure Automatic Serialization</span></span>

<span data-ttu-id="0d2e6-480">Můžete zadat převod strategie, která platí pro každý sloupec s použitím [gson] [ 3] rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-480">You can specify a conversion strategy that applies to every column by using the [gson][3] API.</span></span> <span data-ttu-id="0d2e6-481">Android Klientská knihovna používá [gson] [ 3] na pozadí a serializovat objekty Java do formátu JSON data předtím, než odešle data do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-481">The Android client library uses [gson][3] behind the scenes to serialize Java objects to JSON data before the data is sent to Azure App Service.</span></span>  <span data-ttu-id="0d2e6-482">Následující kód používá **setFieldNamingStrategy()** metoda k nastavení strategie.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-482">The following code uses the **setFieldNamingStrategy()** method to set the strategy.</span></span> <span data-ttu-id="0d2e6-483">Tento příklad odstraní počáteční znak ("m") a pak malá další znak, pro každý název pole.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-483">This example will delete the initial character (an "m"), and then lower-case the next character, for every field name.</span></span> <span data-ttu-id="0d2e6-484">Například ho by zapnout "střední" do "id".</span><span class="sxs-lookup"><span data-stu-id="0d2e6-484">For example, it would turn "mId" into "id."</span></span>  <span data-ttu-id="0d2e6-485">Implementace převod strategie pro snížení nároků na `SerializedName()` poznámky na většina polí.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-485">Implement a conversion strategy to reduce the need for `SerializedName()` annotations on most fields.</span></span>

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

<span data-ttu-id="0d2e6-486">Tento kód musí být spuštěn před vytvořením odkazu mobilního klienta pomocí **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="0d2e6-486">This code must be executed before creating a mobile client reference using the **MobileServiceClient**.</span></span>

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
<span data-ttu-id="0d2e6-487">[Začínáme s ověřováním]: app-service-mobile-android-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="0d2e6-487">[Get started with authentication]: app-service-mobile-android-get-started-users.md</span></span>
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
