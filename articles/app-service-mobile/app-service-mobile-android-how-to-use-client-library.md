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
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a>Jak toouse hello Azure Mobile Apps SDK pro Android

Tento průvodce vám ukáže, jak toouse hello Android klienta SDK pro Mobile Apps tooimplement běžné scénáře, jako například:

* Dotazování na data (vložení, aktualizace a odstranění).
* Ověřování.
* Zpracování chyb.
* Přizpůsobení hello klienta.

Tato příručka se zaměřuje na hello klientské sady SDK pro Android.  Další informace o hello serverové sady SDK pro Mobile Apps naleznete v tématu toolearn [pracovat s .NET back-end SDK] [ 10] nebo [jak toouse hello back-end Node.js SDK] [ 11].

## <a name="reference-documentation"></a>Referenční dokumentace

Můžete najít hello [referenční dokumentace rozhraní API Javadocs] [ 12] pro Android klientské knihovny hello na Githubu.

## <a name="supported-platforms"></a>Podporované platformy

Hello Azure Mobile Apps SDK pro Android podporuje rozhraní API úrovně 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát) pro telefon i tablet velikostem.  Ověřování, zejména využívá běžné webové framework přístup toogather přihlašovací údaje.  Ověřování serveru toku nefunguje s malé formuláře Multi-Factor zařízení, jako jsou sleduje.

## <a name="setup-and-prerequisites"></a>Instalační program a požadavky

Dokončení hello [využít postup rychlého spuštění Mobile Apps](app-service-mobile-android-get-started.md) kurzu.  Tato úloha zajistí, že byly splněny všechny požadavky pro Azure Mobile Apps pro vývoj.  Hello rychlý start také vám pomůže nakonfigurovat svůj účet a vytvoření vaší první back-endu mobilní aplikace.

Pokud se rozhodnete není toocomplete hello rychlý úvodní kurz, dokončete hello následující úlohy:

* [Vytvořte back-end mobilní aplikace] [ 13] toouse s vaší aplikací pro Android.
* V nástroji Android Studio [aktualizace hello Gradle sestavení souborů](#gradle-build).
* [Povolit oprávnění internet](#enable-internet).

### <a name="gradle-build"></a>Aktualizace hello Gradle vytvoření souboru

Obě změnit **build.gradle** soubory:

1. Přidejte tento kód toohello *projektu* úroveň **build.gradle** souboru uvnitř hello *buildscript* značky:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Přidejte tento kód toohello *modulu aplikace* úroveň **build.gradle** souboru uvnitř hello *závislosti* značky:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    Aktuálně hello nejnovější verze je 3.3.0. Hello podporované verze jsou uvedeny [na panelu koše][14].

### <a name="enable-internet"></a>Povolit oprávnění internet

tooaccess Azure, aplikace musí mít povolené oprávnění INTERNET hello. Pokud ještě není povolené, přidejte následující řádek kódu tooyour hello **AndroidManifest.xml** souboru:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>Umožňuje vytvořit připojení klienta

Azure Mobile Apps poskytuje čtyři funkce tooyour mobilních aplikací:

* Přístup k datům a Offline synchronizace s služby mobilní aplikace Azure.
* Volání rozhraní API vlastní napsané pomocí hello Azure Mobile Apps Server SDK.
* Ověřování pomocí služby Azure App Service ověřování a autorizace.
* Registrace nabízených oznámení v Notification Hubs.

Každá z těchto funkcí vyžaduje nejprve vytvoření `MobileServiceClient` objektu.  Pouze jeden `MobileServiceClient` objekt by měl být vytvořen v rámci vašeho mobilního klienta (to znamená, musí být Singleton vzor).  toocreate `MobileServiceClient` objektu:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

Hello `<MobileAppUrl>` je řetězec nebo objekt adresy URL, která bodů mobilního back-endu tooyour.  Pokud používáte Azure App Service toohost mobilního back-endu, pak se ujistěte, použijte zabezpečené hello `https://` verze hello adresy URL.

Hello klient také vyžaduje přístup toohello aktivity nebo kontextu - hello `this` parametr v příkladu hello.  měly by během hello proběhnout Hello konstrukce MobileServiceClient `onCreate()` metoda hello aktivity odkazuje v hello `AndroidManifest.xml` souboru.

Jako osvědčený postup by měl abstraktní komunikace serveru do vlastní třídy (singleton-pattern).  V takovém případě by měla předávat hello aktivity v rámci hello konstruktor tooappropriately konfigurace služby hello.  Například:

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

Nyní můžete volat `AzureServiceAdapter.Initialize(this);` v hello `onCreate()` metoda hlavní činnosti.  Použít jiné metody, kteří potřebují přístup toohello klienta `AzureServiceAdapter.getInstance();` tooobtain adaptérem odkaz toohello služby.

## <a name="data-operations"></a>Operace s daty

Hello jádrem hello Azure Mobile Apps SDK je toodata tooprovide přístup na back-end mobilní aplikace hello uložená v SQL Azure.  Můžete přístup k těmto datům pomocí silného typu třídy (doporučeno) nebo netypová dotazy (nedoporučuje se).  hromadné Hello této části se zabývá pomocí třídy silného typu.

### <a name="define-client-data-classes"></a>Definování datových tříd, klienta

tooaccess data z tabulek SQL Azure, definovat klienta datových tříd, které odpovídají toohello tabulek v back-end mobilní aplikace hello. Příklady v tomto tématu předpokládat tabulku s názvem **MyDataTable**, který má hello následující sloupce:

* id
* Text
* Dokončení

Hello odpovídající typu objektu na straně klienta se nachází v souboru s názvem **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Přidejte metody getter a setter metody pro každé pole, které přidáte.  Pokud SQL Azure tabulka obsahuje více sloupců, měli byste přidat hello odpovídající pole toothis třídy.  Například, pokud hello DTO sloupec s prioritou celé číslo bylo (objekt přenosu dat) a pak může přidejte toto pole, společně s její metody getter a setter metody:

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

toolearn jak zjistit, toocreate dalších tabulek v váš back-end mobilní aplikace [postupy: definování řadič tabulky] [ 15] (.NET back-end) nebo [definovat tabulky pomocí dynamické schématu] [ 16] (Back-end Node.js).

Tabulce back-end Azure Mobile Apps definuje pět speciální pole, čtyři, které jsou k dispozici tooclients:

* `String id`: hello globálně jedinečné ID pro záznam hello.  Jako osvědčený postup, ujistěte se, hello id hello řetězcovou reprezentaci [UUID] [ 17] objektu.
* `DateTimeOffset updatedAt`: hello datum a čas poslední aktualizace hello.  Hello updatedAt pole je nastaven serverem hello a musí být nastavena nikdy váš klientský kód.
* `DateTimeOffset createdAt`: hello datum a čas byl vytvořen tento objekt hello.  Hello createdAt pole je nastaven serverem hello a musí být nastavena nikdy váš klientský kód.
* `byte[] version`: Obvykle vyjádřený jako řetězec, hello verze je také nastavit server hello.
* `boolean deleted`: Označuje, že záznam hello má byla odstraněna ale ještě není vyprázdní.  Nepoužívejte `deleted` jako vlastnost v třídě.

Hello `id` pole je povinné.  Hello `updatedAt` pole a `version` pole se používají pro offline synchronizace (pro přírůstkové synchronizace a dojde ke konfliktu řešení v uvedeném pořadí).  Hello `createdAt` pole je odkaz na pole a není používán hello klienta.  Hello názvy jsou názvy "přes přenosu" hello vlastností a nejsou upravit.  Však vytvořit mapování mezi objektem a hello "přes přenosu" názvy pomocí hello [gson] [ 3] knihovny.  Například:

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

### <a name="create-a-table-reference"></a>Vytvořit odkaz na tabulku

tooaccess tabulku, nejprve vytvořit [MobileServiceTable] [ 8] objekt ve volání hello **jít** metodu hello [MobileServiceClient][9].  Tato metoda má dva přetížení:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

V následující kód, hello **mClient** je MobileServiceClient tooyour referenční objekt.  první přetížení Hello se používá, kde název třídy hello a název tabulky hello jsou hello stejné, a hello jeden slouží v hello rychlý start:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

Hello druhý přetížení se používá při hello název tabulky se liší od názvu třídy hello: první parametr hello je název tabulky hello.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Dotazování tabulky back-end

Nejprve získejte odkaz na tabulku.  Potom spusťte dotaz na odkaz na tabulku hello.  Dotaz je libovolnou kombinaci:

* A `.where()` [klauzuli filtru](#filtering).
* `.orderBy()` [Řazení klauzule](#sorting).
* A `.select()` [klauzule výběr pole](#selection).
* A `.skip()` a `.top()` pro [stránkovaného výsledky](#paging).

klauzule Hello musí uvedené v předcházející pořadí hello.

### <a name="filter"></a>Filtrování výsledků

Obecné formuláře Hello dotazu je:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

Hello předchozí příklad vrací všechny výsledky (až toohello maximální velikost stránky nastavená serverem hello).  Hello `.execute()` metoda provede hello dotaz na back-end hello.  Hello dotaz je převeden tooan [OData v3] [ 19] Dotázat se před přenosem toohello Mobile Apps back-end.  Back-end mobilní aplikace hello na přijetí, převede hello dotazu příkazu SQL před provedením na instanci SQL Azure hello.  Vzhledem k tomu, že nějakou dobu trvá síťové aktivity, hello `.execute()` metoda vrátí [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Filtr vrátil data

Hello následující spuštění dotazu vrátí všechny položky z hello **ToDoItem** tabulky kde **dokončení** rovná **false**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** je hello referenční toohello mobilní služby tabulku, kterou jsme vytvořili dříve.

Definujte filtr pomocí hello **kde** volání metody na odkaz na tabulku hello. Hello **kde** metoda následuje **pole** metoda následuje metodu, která určuje hello logické predikátu. Zahrnout možných metod predikátem **eq** (rovná), **ne** (nerovná), **gt** (větší než), **ge** (větší než nebo rovno), **lt** (méně než), **le** (je menší než nebo rovno). Tyto metody umožňují porovnat číslo a řetězec polí toospecific hodnoty.

Můžete filtrovat podle data. Hello následující metody umožňují porovnat pole pro celý datum hello nebo její části datum hello: **roku**, **měsíc**, **den**, **hodinu**, **minutu**, a **druhý**. Hello následující příklad přidá filtr pro položky jejichž *datum splatnosti* rovná 2013.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

Hello následující metody podporují komplexní filtry na polí s řetězcem: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **nahradit**, **toLower**, **toUpper**, **trim**, a  **Délka**. Následující příklad filtry pro tabulku řádků, kde hello Hello *text* sloupec začíná "PRI0."

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

podporovány jsou Hello následující metody operátor pro pole s počtem: **přidat**, **sub**, **mul**, **div**, **mod**, **podlaží**, **mezní hodnoty**, a **ZAOKROUHLIT**. Následující příklad filtry pro tabulku řádků, kde hello Hello **doba trvání** je číslo sudé.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

Predikáty můžete kombinovat s tyto logické metody: **a**, **nebo** a **není**. Následující ukázka Hello kombinuje dvě hello předcházející příklady.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Skupiny a vnořit logické operátory:

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

Podrobnější diskuzi a příklady filtrování, najdete v části [zkoumat hello bohatost modelu dotazu Android klienta hello][20].

### <a name="sorting"></a>Řazení vrátil data

Hello následující kód vrátí všechny položky z tabulky **ToDoItems** seřadit vzestupně podle hello *text* pole. *mToDoTable* je hello referenční toohello back-end tabulku, kterou jste vytvořili dříve:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

první parametr hello Hello **orderBy** metoda je rovna toohello názvu řetězce hello pole, na které toosort. druhý parametr Hello používá hello **QueryOrder** výčtu toospecify zda toosort vzestupném nebo sestupném.  Filtrování pomocí hello ***kde*** metoda, hello ***kde*** metoda musí být volána před hello ***orderBy*** metoda.

### <a name="selection"></a>Vyberte konkrétní sloupce

Hello následující kód ukazuje, jak tooreturn všechny položky z tabulky **ToDoItems**, ale zobrazuje jenom hello **dokončení** a **text** pole. **mToDoTable** je hello referenční toohello back-end tabulku, kterou jsme vytvořili dříve.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

Vyberte funkce toohello Hello parametry jsou hello řetězec názvy sloupců hello tabulky, které chcete tooreturn.  Hello **vyberte** toofollow metody, třeba musí metoda **kde** a **orderBy**. Může následovat stránkování metody, třeba **přeskočit** a **horní**.

### <a name="paging"></a>Vrátit data na stránkách

Data jsou **vždy** vrátil na stránkách.  maximální počet záznamů vrácených Hello nastavena serverem hello.  Pokud hello klient požádá o další záznamy, vrátí hello server hello maximální počet záznamů.  Ve výchozím nastavení je hello maximální velikost stránky na hello server 50 záznamů.

Hello první příklad ukazuje, jak tooselect hello prvních pět položek z tabulky. Hello dotaz vrátí hello položky z tabulky **ToDoItems**. **mToDoTable** je hello referenční toohello back-end tabulku, kterou jste vytvořili dříve:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Zde je dotaz přeskočí hello prvních pět položek, a potom vrátí hello další pět:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Pokud chcete tooget všechny záznamy v tabulce implementaci kódu tooiterate přes všechny stránky:

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

Žádost o pro všechny záznamy pomocí této metody vytvoří minimálně dva požadavky toohello Mobile Apps back-end.

> [!TIP]
> Výběrem možnosti velikost pravá stránka hello je rovnováhu mezi využití paměti při žádosti o hello se děje, využití šířky pásma a zpoždění při příjmu dat hello úplně.  Výchozí Hello (50 záznamy) je vhodná pro všechna zařízení.  Pokud provozujete výhradně na větší paměti zařízení, zvýšit až too500.  Našli jsme tato roste velikost stránky hello nad rámec 500 zaznamenává výsledky v zpožděním a velké paměti problémy.

### <a name="chaining"></a>Postupy: řetězení metody dotazů

může být zřetězen Hello metody používané v dotazování tabulky back-end. Řetězení dotazu metody vám umožní tooselect určité sloupce filtrované řádků, které jsou seřazené a stránkovaného fondu. Můžete vytvořit komplexní logické filtry.  Každá metoda dotaz vrátí objekt dotazu. tooend hello řady metod a ve skutečnosti spuštění hello dotaz, volání hello **provést** metoda. Například:

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

Hello zřetězené dotaz, který metody musejí být seřazeny následujícím způsobem:

1. Filtrování (**kde**) metody.
2. Řazení (**orderBy**) metody.
3. Výběr (**vyberte**) metody.
4. stránkování (**přeskočit** a **horní**) metody.

## <a name="binding"></a>Vytvoření vazby dat toohello uživatelské rozhraní

Datová vazba zahrnuje tři komponenty:

* zdroj dat Hello
* rozložení obrazovky Hello
* adaptér Hello že ties hello dva společně.

V našem ukázkový kód vrátíme hello data z tabulky Mobile Apps SQL Azure hello **ToDoItem** na pole. Tato aktivita je běžný vzor pro data aplikací.  Databázové dotazy často vracet kolekci řádků, které hello klient získá v seznamu nebo pole. V této ukázce hello pole je zdroj dat hello.  Kód Hello určuje rozložení obrazovky, který definuje zobrazení hello hello dat, který se zobrazí na zařízení hello.  Hello dva jsou svázané s společně s adaptér, který tento kód je rozšířením hello **ArrayAdapter&lt;ToDoItem&gt;**  třídy.

#### <a name="layout"></a>Definování hello rozložení

rozložení Hello je definována několik fragmenty kódu XML. Existující rozložení zadána hello následující kód představuje hello **ListView** chceme toopopulate pomocí našich dat serveru.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

V předchozích kód hello, hello *listitem* atribut určuje hello id hello rozložení jednotlivých řádků v seznamu hello. Tento kód určuje zaškrtávací políčko a jeho přidružené textu a získá instanci jednou pro každou položku v seznamu hello. Toto rozložení nezobrazí hello **id** pole a složitější rozložení by zadejte další pole v zobrazení hello. Tento kód je v hello **row_list_to_do.xml** souboru.

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

#### <a name="adapter"></a>Zadejte adaptér hello
Vzhledem k tomu, že zdroj dat hello naše zobrazení je pole **ToDoItem**, jsme podtřídami adaptéru v našem **ArrayAdapter&lt;ToDoItem&gt;**  třídy. Tato podtřídami vytvoří zobrazení pro každý **ToDoItem** pomocí hello **row_list_to_do** rozložení.  V našem kódu jsme definovali hello následující třídy, která je rozšířením hello **ArrayAdapter&lt;E&gt;**  třídy:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Přepsání hello adaptéry **getView** metoda. Například:

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

Vytvoříme instance této třídy v našem aktivity následujícím způsobem:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

Hello druhý parametr toohello ToDoItemAdapter konstruktor je rozložení toohello odkaz. Jsme teď vytvořit instanci hello **ListView** a přiřaďte hello adaptér toohello **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Použít hello adaptér tooBind toohello uživatelského rozhraní

Nyní je připraven toouse datové vazby. Hello následující kód ukazuje, jak tooget položky v tabulce hello a výplněmi hello místní adaptéru s hello vrátil položky.

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

Volání hello adaptér kdykoli upravit hello **ToDoItem** tabulky. Vzhledem k tomu, že změny se provádí na základě záznamu podle, můžete zpracovat jeden řádek místo kolekce. Při vložení položku volání hello **přidat** metoda na hello adaptér; při odstraňování, volání hello **odebrat** metoda.

Úplný příklad můžete najít v hello [projekt Android rychlý Start][21].

## <a name="inserting"></a>Vložení dat do back-end hello

Vytvoří instanci hello *ToDoItem* a nastavit jeho vlastnosti.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Potom pomocí **insert()** tooinsert objekt:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

Hello vrátil entity odpovídá hello data vložená do tabulky hello back-end, zahrnuté hello ID a všechny ostatní hodnoty (například hello `createdAt`, `updatedAt`, a `version` polí) nastavte na back-end hello.

Mobile Apps tabulky vyžadují sloupec primárního klíče s názvem **id**. Tento sloupec musí být řetězec. Výchozí hodnota Hello hello ID sloupce je identifikátor GUID.  Můžete zadat další jedinečné hodnoty, například e-mailové adresy nebo uživatelských jmen. Pokud není zadána hodnota ID řetězec vloženého záznamu, back-end hello generuje nový identifikátor GUID.

Řetězcové hodnoty ID poskytují hello následující výhody:

* ID může být generována bez provedení databáze toohello odezvy.
* Záznamy jsou jednodušší toomerge z různých tabulek nebo databází.
* Hodnoty ID lépe integrovat logiku aplikace.

Řetězec ID hodnoty jsou **REQUIRED** pro podporu offline synchronizace.  Id nelze změnit, jakmile je uložen v databázi back-end hello.

## <a name="updating"></a>Aktualizovat data v mobilní aplikaci

tooupdate data v tabulce, předat hello nový objekt toohello **update()** metoda.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

V tomto příkladu *položky* je odkaz na řádek tooa v hello *ToDoItem* tabulku, která se použila tooit některé změny provedené v.  Hello řádek s hello stejné **id** se aktualizuje.

## <a name="deleting"></a>Odstranit data v mobilní aplikaci

Hello následující kód ukazuje, jak toodelete data z tabulky zadáním hello datový objekt.

```java
mToDoTable
    .delete(item);
```

Můžete také odstranit položku zadáním hello **id** pole z toodelete řádek hello.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Vyhledat konkrétní položky podle Id

Vyhledání položky s konkrétní **id** pole s hello **lookUp()** metoda:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Postupy: práce s daty bez typu

netypové programovací model Hello vám dává přesnou kontrolu nad serializace JSON.  Existují některé běžné scénáře, kde můžete toouse netypové programovací model. Například pokud back-end tabulka obsahuje mnoho sloupců a potřebujete jenom tooreference podmnožinu sloupců hello.  Hello typu modelu vyžaduje toodefine všechny hello sloupce definované v back-end mobilní aplikace hello v třídě data.  Většina hello volání rozhraní API pro přístup k datům jsou podobné toohello zadali programovací volání. Hello hlavní rozdíl je, že v modelu netypové hello volat metody na hello **MobileServiceJsonTable** objektu, nikoli hello **MobileServiceTable** objektu.

### <a name="json_instance"></a>Vytvoření instance bez typu tabulky

Podobné toohello typu modelu, můžete začít nastavením odkaz na tabulku, ale v takovém případě je **MobileServicesJsonTable** objektu. Získejte odkaz na hello volání hello **jít** metoda na instanci hello klienta:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Po vytvoření instance hello **MobileServiceJsonTable**, ho má prakticky hello stejné rozhraní API k dispozici jako s typem programovací model hello. V některých případech hello metody přijímají bez typu parametru místo typu parametru.

### <a name="json_insert"></a>Vložit do tabulky bez typu
Následující kód ukazuje, jak Hello toodo typu vložení. Hello prvním krokem je toocreate [JsonObject][1], který je součástí hello [gson] [ 3] knihovny.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Poté použijte **insert()** tooinsert hello bez typu objektu do tabulky hello.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Pokud potřebujete tooget hello ID objektu hello vložit, použijte hello **getAsJsonPrimitive()** metoda.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Odstraňte z bez typu tabulky
Hello následující kód ukazuje, jak toodelete na instance, v takovém případě hello stejnou instanci **JsonObject** který byl vytvořen v hello před *vložit* příklad. Kód Hello je hello stejné jako u hello zadali případ, ale metoda hello má jiný podpis, protože odkazuje na **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

Můžete také odstranit instanci přímo pomocí jeho ID:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Vrátí všechny řádky z tabulky aplikace bez typu
Následující kód ukazuje, jak Hello tooretrieve celou tabulku. Vzhledem k tomu, že používáte JSON tabulky, můžete selektivně načíst jenom některé sloupce tabulky hello.

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

Hello stejnou sadu filtrování, filtrování a stránkování metody, které jsou k dispozici pro hello typu modelu jsou k dispozici pro hello bez typu modelu.

## <a name="offline-sync"></a>Implementace Offline synchronizace

Hello Azure Mobile Apps Client SDK také implementuje offline synchronizace dat s použitím toostore databáze SQLite kopii dat serveru hello místně.  Operace provedené v offline tabulce nevyžadují toowork mobilní připojení.  Offline synchronizace je výhodné při odolnost a výkon při hello nákladů na složitější logiku pro řešení konfliktů.  Hello Azure Mobile Apps Client SDK implementuje hello následující funkce:

* Přírůstkové synchronizace: Pouze aktualizované a nové záznamy se stáhnou, ukládání spotřebu šířky pásma a paměti.
* Optimistickou metodu souběžného: Operace se předpokládá, že toosucceed.  Řešení konfliktů je odložení, dokud nebude aktualizace se na hello server.
* Řešení konfliktů: hello SDK zjistí, že změna způsobující konflikt byl na hello server a poskytuje zachytí tooalert hello uživatele.
* Obnovitelného odstranění: Odstraněné záznamy jsou označené odstraněné, povolení dalších zařízení tooupdate offline mezipaměti.

### <a name="initialize-offline-sync"></a>Inicializaci Offline synchronizace

Každá tabulka offline musí být definován v mezipaměti offline hello před použitím.  Za normálních okolností se okamžitě po vytvoření hello hello klienta provádí definici tabulky:

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

### <a name="obtain-a-reference-toohello-offline-cache-table"></a>Získat odkaz na toohello Offline tabulky mezipaměti

Pro online tabulky, můžete použít `.getTable()`.  K offline tabulky, použijte `.getSyncTable()`:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

Všechny metody, které jsou k dispozici pro online tabulky (včetně filtrování, řazení, stránkování, vkládání dat, aktualizace dat a odstraňování dat) pracovat stejně hello i na tabulkách online a offline.

### <a name="synchronize-hello-local-offline-cache"></a>Synchronizovat hello místní mezipaměti v režimu Offline

Synchronizace je v rámci ovládacího prvku hello vaší aplikace.  Tady je příklad metoda synchronizace:

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

Pokud název dotazu je zadaný toohello `.pull(query, queryname)` metoda pak přírůstkové synchronizace je použité tooreturn pouze záznamy, které byly vytvořené nebo změněné od vyžadování hello poslední byla úspěšně dokončena.

### <a name="handle-conflicts-during-offline-synchronization"></a>Zpracování konfliktů během Offline synchronizace

Pokud dojde ke konfliktu při `.push()` operace, `MobileServiceConflictException` je vyvolána výjimka.   Položka server vydal Hello vložené v hello výjimek a může načíst `.getItem()` na hello výjimce.  Upravte hello nabízené pomocí volání hello u objektu MobileServiceSyncContext hello následující položky:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

Jakmile se všechny konflikty jsou označené jako nechcete, volání `.push()` znovu tooresolve všechny hello je v konfliktu.

## <a name="custom-api"></a>Volání vlastní rozhraní API

Vlastní rozhraní API umožňuje toodefine vlastní koncové body, které zveřejňují funkce serveru které není mapování tooan vložení, aktualizaci, odstranění nebo operace čtení. Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.

V klientovi aplikace Android volání hello **invokeApi** metoda toocall hello vlastní koncový bod rozhraní API. Hello následující příklad ukazuje, jak toocall koncový bod rozhraní API s názvem **completeAll**, který vrátí kolekce třídy s názvem **MarkAllResult**.

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

Hello **invokeApi** metoda je volána na hello klienta, který odesílá příspěvku na žádosti toohello nové vlastní rozhraní API. Hello výsledek vrácený vlastního rozhraní API hello zobrazí v dialogu zprávy, jako jsou všechny chyby. Jiné verze **invokeApi** umožňují volitelně odesílat objekt v textu žádosti hello, zadejte metodu hello HTTP a odeslat parametry dotazu hello požadavku. Netypová verze **invokeApi** k dispozici jsou také.

## <a name="authentication"></a>Přidat aplikaci tooyour ověřování

Kurzy již podrobně popisují, jak tooadd tyto funkce.

App Service podporuje [ověřování uživatelů aplikace](app-service-mobile-android-get-started-users.md) pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory. Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele. Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel v váš back-end.

Jsou podporovány dva ověřování toky: **server** toku a **klienta** toku. Vývojový server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello identity poskytovatelů webové rozhraní.  Žádné další sady SDK jsou požadované tooimplement serveru tok ověřování. Tok ověřování serveru neposkytuje těsná integrace do mobilních zařízení hello a doporučuje se pouze pro testování konceptu scénáře.

tok klienta Hello umožňuje hlubší integrace s funkcí konkrétní zařízení, jako je jednotné přihlašování jako přitom spoléhá na sady SDK poskytované poskytovatelem identity hello.  Například můžete integrovat hello Facebook SDK do své mobilní aplikace.  Hello mobilního klienta umožňuje přepnout do aplikace Facebook hello a potvrdí, vaše přihlášení před odkládací back tooyour mobilní aplikace.

Čtyři kroky jsou požadované tooenable ověřování ve vaší aplikaci:

* Registrace aplikace pro ověřování pomocí zprostředkovatele identity.
* Konfigurace back-end vaší služby App Service.
* Umožňuje omezte uživatele tooauthenticated tabulky oprávnění pouze na hello back-end služby App Service.
* Přidáte aplikaci tooyour kód ověřování.

Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele. Můžete taky hello SID ověřenému uživateli toomodify požadavky.  Další informace najdete v tématu [Začínáme s ověřováním] a hello serveru SDK postupy dokumentaci.

### <a name="caching"></a>Ověřování: Tok serveru

Hello následující kód spustí proces serveru toku přihlášení pomocí zprostředkovatele hello Google.  Další konfigurace je vyžadován kvůli hello požadavky zabezpečení pro zprostředkovatele Google hello:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Kromě toho přidejte následující hlavní třída aktivit metoda toohello hello:

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

Hello `GOOGLE_LOGIN_REQUEST_CODE` definované v vaší hlavní aktivita se používá pro hello `login()` metoda a v rámci hello `onActivityResult()` metoda.  Jedinečné číslo, můžete tak dlouho, dokud hello se používá stejné číslo v rámci hello `login()` metoda a hello `onActivityResult()` metoda.  Pokud jste abstraktní hello kód klienta do služby adaptér (jak je uvedeno výše), by měly volat metody odpovídající hello na adaptéru služby hello.

Budete také potřebovat pro customtabs tooconfigure hello projektu.  Nejdřív zadejte adresu URL přesměrování.  Přidejte následující fragment kódu příliš hello`AndroidManifest.xml`:

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

Přidat hello **redirectUriScheme** toohello `build.gradle` souboru aplikace:

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

Nakonec přidejte `com.android.support:customtabs:23.0.1` toohello aplikací v hello `build.gradle` souboru:

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

Získat ID hello hello přihlášeného uživatele z **MobileServiceUser** pomocí hello **reprezentuje getUserId** metoda. Příklad jak toouse Futures toocall hello asynchronní přihlášení rozhraní API, naleznete v části [Začínáme s ověřováním].

> [!WARNING]
> Hello schéma adresy URL uvedené rozlišuje velká a malá písmena.  Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` malá a velká písmena.

### <a name="caching"></a>Tokeny ověřování do mezipaměti

Ukládání do mezipaměti tokeny ověřování vyžaduje toostore hello ID uživatele a ověřovací token místně na hello zařízení. Hello příštím spuštění aplikace hello, můžete zkontrolujte hello mezipaměti, a pokud nejsou tyto hodnoty, můžete přeskočit hello protokolu v postupu a rehydrataci při spotřebě hello klienta s těmito daty. Ale tato data jsou citlivé a by měly být uložené šifrována pro zabezpečení v případě, že získá odcizení hello phone.  Zobrazí kompletní příklad, jak jak toocache ověřování tokenů v [mezipaměti část tokeny ověřování][7].

Když zkusíte toouse tokenu vypršela platnost, zobrazí se *401 Neautorizováno* odpovědi. Může zpracovávat ověřování chyb pomocí filtrů.  Filtry intercept požadavky toohello back-end služby App Service. kód filtru Hello testy hello odpovědi na 401, aktivuje hello přihlašovací proces a potom obnoví hello žádosti, která vygenerovala hello 401.

### <a name="refresh"></a>Použití obnovovacích tokenů

Hello token vrácený Azure App Service ověřování a autorizace má definovaná životnosti jednu hodinu.  Po uplynutí této doby musí novému ověření uživatele hello.  Pokud jste dlohotrvající token, který jste obdrželi prostřednictvím ověřování tok klienta a pak můžete novému ověření pomocí Azure App Service ověřování a autorizace pomocí hello stejný token.  Další token služby Azure App Service je vytvořen s novou životnost.

Můžete také registrovat toouse hello poskytovatele aktualizaci tokeny.  Aktualizovat Token není vždy k dispozici.  Je vyžadována další konfigurace:

* Pro **Azure Active Directory**, nakonfigurujte tajný klíč klienta pro hello aplikaci služby Azure Active Directory.  Zadejte sdílený tajný klíč klienta hello v hello Azure App Service při konfiguraci ověřování Azure Active Directory.  Při volání metody `.login()`, předat `response_type=code id_token` jako parametr:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Pro **Google**, předat hello `access_type=offline` jako parametr:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Pro **Account Microsoft**, vyberte hello `wl.offline_access` oboru.

volání toorefresh token, `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

Jako osvědčený postup vytvoření filtru, který zjistí 401 odpověď ze serveru hello a pokusí toorefresh hello uživatelského tokenu.

## <a name="log-in-with-client-flow-authentication"></a>Přihlaste se pomocí ověřování tok klienta

Obecný postup Hello přihlášení pomocí ověřování tok klienta vypadá takto:

* Konfigurace Azure App Service ověřování a autorizaci, stejně jako server tok ověřování.
* Integrate zprostředkovatele ověřování hello SDK pro ověřování tooproduce přístupový token.
* Volání hello `.login()` metoda následujícím způsobem:

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

Nahraďte hello `onSuccess()` metoda s ať vám kód chcete toouse na úspěšného přihlášení.  Hello `{provider}` řetězec je platný zprostředkovatel: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, nebo **twitter**.  Pokud jste implementovali vlastní ověřování, můžete také použít hello vlastního ověřování zprostředkovatele značky.

### <a name="adal"></a>Ověřuje uživatele pomocí hello Active Directory Authentication Library (ADAL)

Můžete vytvořit hello Active Directory Authentication Library (ADAL) toosign uživatelů do vaší aplikace pomocí Azure Active Directory. Pomocí přihlášení toku klienta je často vhodnější toousing hello `loginAsync()` metody jak poskytuje více nativní UX chování a umožňuje pro další přizpůsobení.

1. Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] [ 22] kurzu. Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci.
2. Nainstalujte ADAL změnou vaší hello tooinclude souboru build.gradle následující definice:

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

1. Přidejte následující kód tooyour aplikace, která hello následující náhrady hello:

* Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace. Hello formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com.
* Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace. ID klienta hello můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.
* Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.
* Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS. Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.

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

## <a name="filters"></a>Upravit hello komunikaci klienta se serverem

Hello připojení klienta je obvykle základní připojení HTTP pomocí hello základní knihovna HTTP dodává s hello sady SDK pro Android.  Tady je několik důvodů, proč byste měli toochange který:

* Chcete toouse alternativní HTTP knihovny tooadjust vypršení časových limitů.
* Chcete tooprovide indikátor průběhu.
* Chcete tooadd funkce správy toosupport rozhraní API vlastní hlavičky.
* Chcete toointercept neúspěšných odpovědí proto, že můžete implementovat opětovné ověření.
* Chcete toolog back-end požadavky tooan analytics službu.

### <a name="using-an-alternate-http-library"></a>Pomocí alternativní knihovny HTTP

Volání hello `.setAndroidHttpClientFactory()` metoda ihned po vytvoření odkaz na klienta.  Například tooset hello připojení časový limit too60 v sekundách (namísto výchozí hello 10 sekund):

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

### <a name="implement-a-progress-filter"></a>Implementace filtru průběh

Zachycení každou žádost můžete implementovat implementací `ServiceFilter`.  Například následující hello aktualizací indikátor průběhu předem vytvořené:

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

Tento klient toohello filtru můžete připojit následujícím způsobem:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>Přizpůsobení hlavičky požadavku

Použijte hello `ServiceFilter` a připojte hello filtru v hello stejným způsobem jako hello `ProgressFilter`:

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

### <a name="conversions"></a>Konfigurace automatického serializace

Můžete zadat převod strategie, která se použije tooevery sloupce s použitím hello [gson] [ 3] rozhraní API. Hello Android Klientská knihovna používá [gson] [ 3] pozadí hello tooserialize Java objekty tooJSON dat před odesláním dat hello tooAzure služby App Service.  Hello následující kód používá hello **setFieldNamingStrategy()** metoda tooset hello strategie. Tento příklad odstraní hello počáteční znak ("m") a pak malá hello další znak, pro každý název pole. Například ho by zapnout "střední" do "id".  Implementace převod strategie tooreduce hello nutnost `SerializedName()` poznámky na většina polí.

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

Tento kód musí být spuštěn před vytvořením mobilního klienta odkaz pomocí hello **MobileServiceClient**.

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
