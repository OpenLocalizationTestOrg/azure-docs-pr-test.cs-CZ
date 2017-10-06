---
title: aaaCopy tooand dat z Azure Data Lake Store | Microsoft Docs
description: "Zjistěte, jak toocopy tooand dat z Data Lake Store pomocí Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a>Zkopírujte tooand dat z Data Lake Store pomocí objektu pro vytváření dat
Tento článek vysvětluje, jak toouse aktivitu kopírování v Azure Data Factory toomove data tooand z Azure Data Lake Store. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku přehled o přesun dat s aktivitou kopírování.

## <a name="supported-scenarios"></a>Podporované scénáře
Může kopírovat data **z Azure Data Lake Store** toohello následující úložišť dat:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Data můžete zkopírovat z hello následující úložišť dat **tooAzure Data Lake Store**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Před vytvořením kanálu s aktivitou kopírování, vytvoření účtu Data Lake Store. Další informace najdete v tématu [Začínáme s Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Typy podporované ověřování
konektor Hello Data Lake Store podporuje tyto typy ověřování:
* Ověřování instančních objektů
* Ověření přihlašovacích údajů (OAuth) uživatele 

Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánované dat kopie. Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele. Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.

## <a name="get-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Data Lake Store pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello toocopy datový kanál je toouse hello **Průvodce kopírováním**. Kurz týkající se vytváření kanálu pomocí Průvodce kopírováním hello, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů. 
2. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory. Například pokud kopírujete data ze Azure blob storage tooan Azure Data Lake Store, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure Data Lake store tooyour data factory. Vlastnosti propojené služby, které jsou specifické tooAzure Data Lake Store, naleznete v části [propojené vlastnosti služby](#linked-service-properties) části. 
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello. A vytvořte jinou datovou sadu toospecify hello složky a cestu k souboru v úložišti Data Lake hello, který obsahuje hello daty zkopírovanými z úložiště objektů blob hello. Vlastnosti datové sady, které jsou specifické tooAzure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a AzureDataLakeStoreSink jako jímku pro aktivitu kopírování hello. Podobně pokud zkopírujete z Azure Data Lake Store tooAzure úložiště objektů Blob, použijte AzureDataLakeStoreSource a BlobSink v aktivitě kopírování hello. Vlastnosti aktivity kopírování, které jsou specifické tooAzure Data Lake Store, naleznete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.  

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo ze Azure Data Lake Store naleznete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-data-lake-store) tohoto článku.

Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooData Lake Store.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Propojená služba odkazuje data store tooa objekt pro vytváření dat. Vytvoření propojené služby typu **AzureDataLakeStore** toolink Data Lake Store data tooyour datovou továrnu. Hello následující tabulka popisuje JSON elementy konkrétní tooData Lake Store propojené služby. Můžete zvolit instanční objekt a ověření přihlašovacích údajů uživatele.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **Typ** | musí být nastavena vlastnost typu Hello příliš**AzureDataLakeStore**. | Ano |
| **dataLakeStoreUri** | Informace o hello účtu Azure Data Lake Store. Tyto informace má jednu z následujících formátů hello: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`. | Ano |
| **ID předplatného** | Předplatné Azure ID toowhich hello účtu Data Lake Store patří. | Vyžaduje se pro sink |
| **Název skupiny prostředků** | Patří prostředků Azure skupiny název toowhich hello účtu Data Lake Store. | Vyžaduje se pro sink |

### <a name="service-principal-authentication-recommended"></a>Objekt zabezpečení ověřování služby (doporučeno)
objekt zabezpečení ověřování služby toouse registrace entity aplikace v Azure Active Directory (Azure AD) a udělte ho hello přístup tooData Lake Store. Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Poznamenejte si hello následující hodnoty, které používáte toodefine hello propojené služby:
* ID aplikace
* Klíč aplikace 
* ID tenanta

> [!IMPORTANT]
> Pokud používáte hello Průvodce kopírováním tooauthor datových kanálů, ujistěte se, že udělíte hello instanční objekt alespoň **čtečky** role v řízení přístupu (správu identit a přístupu) pro účet Data Lake Store hello. Navíc alespoň udělit hello instanční objekt **čtení + Execute** oprávnění tooyour Data Lake Store kořenové ("/") a její podřízené položky. V opačném případě může zobrazit uvítací zprávu "hello, poskytnuté přihlašovací údaje jsou neplatné."<br/><br/>
Po vytvoření nebo aktualizaci objektu služby ve službě Azure AD, může trvat několik minut, než platnost tootake změny hello. Zkontrolujte hello instanční objekt a Data Lake Store přístup ovládacího prvku seznam (ACL) konfigurace. Pokud stále vidět uvítací zprávu "hello, poskytnuté přihlašovací údaje jsou neplatné", Vyčkejte chvíli a zkuste to znovu.

Objekt zabezpečení ověřování služby použijte zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **servicePrincipalId** | Zadejte ID aplikace hello klienta. | Ano |
| **servicePrincipalKey** | Zadejte klíč aplikace hello. | Ano |
| **klienta** | Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure. | Ano |

**Příkladu: Ověření objektu službu**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Ověření pověření uživatele
Alternativně můžete použít toocopy ověřování přihlašovacích údajů uživatele z nebo tooData Lake Store zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **autorizace** | Klikněte na tlačítko hello **Autorizovat** tlačítka na hello editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky vygenerovanou autorizace URL toothis vlastnost. | Ano |
| **ID relace** | ID relace OAuth z autorizační relace, hello OAuth. Každé ID relace je jedinečné a může být použit pouze jednou. Toto nastavení se automaticky generuje při použití hello editoru služby Data Factory. | Ano |

**Příklad: Ověření pověření uživatele**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Hello autorizační kód, který můžete vygenerovat pomocí hello **Autorizovat** tlačítko vyprší po určité době. Hello následující zpráva znamená, že vypršela platnost tohoto hello ověřovací token:

Přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů. AADSTS70008: hello údaje udělení přístupu vypršela platnost nebo odvolat. ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21-09-31Z.

Hello následující tabulka uvádí dobu vypršení platnosti hello různé typy uživatelských účtů:


| Typ uživatele | Platnost vyprší po |
|:--- |:--- |
| Uživatelské účty *není* spravované službou Azure Active Directory (například @hotmail.com nebo @live.com) |12 hodin |
| Účty uživatelů spravované službou Azure Active Directory |14 dnů po poslední řez hello spustit <br/><br/>90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní |

Pokud změníte heslo před hello dobu vypršení platnosti tokenu, platnost tokenu hello okamžitě vyprší. Zobrazí se zpráva hello dříve popsaných v této části.

Můžete opětovné pověření účtu hello pomocí hello **Autorizovat** tlačítko když hello tokenu vyprší platnost tooredeploy hello propojené služby. Můžete také vygenerovat hodnoty pro hello **sessionId** a **autorizace** vlastnosti programově pomocí hello následující kód:


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Podrobnosti o třídy hello Data Factory používané v hello kódu najdete v tématu hello [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [ Třída AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata. Přidat odkaz na tooversion `2.9.10826.1824` z `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` pro hello `WindowsFormsWebAuthenticationDialog` třída používaná v kódu hello.

## <a name="dataset-properties"></a>Vlastnosti datové sady
toospecify datovou sadu toorepresent vstupních dat v Data Lake Store, nastavíte hello **typ** vlastnosti datové sady hello příliš**AzureDataLakeStore**. Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Data Lake Store propojené služby. Úplný seznam části JSON a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Části datové sady ve formátu JSON, jako například **struktura**, **dostupnosti**, a **zásad**, jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, databáze a tabulky Azure, např.). Hello **rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace, jako je například umístění a formát hello dat v úložišti dat hello. 

Hello **rámci typeProperties** části datové sady typu **AzureDataLakeStore** obsahuje hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **folderPath** |Cesta toohello kontejneru a složce v Data Lake Store. |Ano |
| **Název souboru** |Název souboru hello v Azure Data Lake Store. Hello **fileName** vlastnost je volitelná a velká a malá písmena. <br/><br/>Pokud zadáte **fileName**, na konkrétní soubor hello funguje hello aktivitu (včetně kopie).<br/><br/>Když **fileName** není zadán, zahrnuje kopírování všech souborů v **folderPath** v hello vstupní datové sady.<br/><br/>Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený hello název souboru hello generované je ve formátu hello Data. _Identifikátor GUID_.txt'. Například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Ne |
| **partitionedBy** |Hello **partitionedBy** vlastnost je nepovinná. Můžete ho toospecify dynamické cestu a název souboru pro data časové řady. Například **folderPath** lze nastavit parametry pro každou hodinu data. Podrobnosti a příklady naleznete v tématu [hello vlastnost partitionedBy](#using-partitionedby-property). |Ne |
| **Formát** | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, a  **ParquetFormat**. Sada hello **typ** vlastnost pod **formátu** tooone z těchto hodnot. Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) části hello [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku. <br><br> Pokud chcete soubory toocopy "jako-je" mezi souborové úložiště (binární kopie), přeskočte hello `format` část v obou definice vstupní a výstupní datové sady. |Ne |
| **komprese** | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

### <a name="hello-partitionedby-property"></a>Vlastnost partitionedBy Hello
Můžete zadat dynamické **folderPath** a **fileName** vlastnosti pro data časové řady s hello **partitionedBy** vlastnost, funkce pro vytváření dat a systému proměnné. Podrobnosti najdete v tématu hello [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku.


V následujícím příkladu hello `{Slice}` se nahradí hello hodnoty proměnné systému Data Factory hello `SliceStart` v zadaný formát hello (`yyyyMMddHH`). Název Hello `SliceStart` odkazuje čas zahájení toohello hello řez. Hello `folderPath` vlastnost je jiný pro každý řez, jako v `wikidatagateway/wikisampledataout/2014100103` nebo `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

V následujícím příkladu, hello rok, měsíc, den a čas hello `SliceStart` extrahují do samostatné proměnné, které jsou používány hello `folderPath` a `fileName` vlastnosti:
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Další informace o datové sady času series, plánování a řezů, najdete v části hello [datové sady v Azure Data Factory](data-factory-create-datasets.md) a [Data Factory plánování a provádění](data-factory-scheduling-and-execution.md) články. 


## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Hello vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

**AzureDataLakeStoreSource** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| **rekurzivní** |Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky. |True (výchozí hodnota), False. |Ne |


**AzureDataLakeStoreSink** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| **copyBehavior** |Určuje chování kopie hello. |<b>PreserveHierarchy</b>: zachovává hello hierarchií souborů v cílové složce hello. relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.<br/><br/><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou vytvořené v první úroveň hello hello cílové složce. Hello zaměřením jsou vytvořeny s názvy generován automaticky.<br/><br/><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru. Pokud hello soubor nebo objekt blob je zadán název, název sloučené souboru hello je zadaný název hello. Název souboru hello jinak, je generován automaticky. |Ne |

### <a name="recursive-and-copybehavior-examples"></a>Příklady rekurzivní a copyBehavior
Tato část popisuje hello výsledné chování hello kopírování pro různé kombinace hodnot rekurzivní a copyBehavior.

| Rekurzivní | copyBehavior | Výsledné chování |
| --- | --- | --- |
| Hodnota TRUE |preserveHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello stejné struktury jako zdroj hello<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| Hodnota TRUE |flattenHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Vytvoření cíle Hello složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5 |
| Hodnota TRUE |mergeFiles |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Vytvoření cíle Hello složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor |
| False |preserveHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |flattenHierarchy |Pro zdrojové složky složku1 s hello strukturu:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/><br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |mergeFiles |Pro zdrojové složky složku1 s hello strukturu:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor. automaticky generovaný název File1<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |

## <a name="supported-file-and-compression-formats"></a>Podporované formáty souborů a komprese
Podrobnosti najdete v tématu hello [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku.

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a>Příklady JSON kopírování tooand dat z Data Lake Store
Následující příklady Hello poskytují definice JSON ukázka. Můžete použít tyto ukázkové definice toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Zobrazit příklady jak Hello toocopy tooand dat z Data Lake Store a Azure Blob storage. Nicméně je možné zkopírovat data _přímo_ z jakéhokoli z tooany zdroje hello z jímky hello podporována. Další informace najdete v tématu hello části "podporované úložiště dat a formáty" v hello [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md) článku.  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a>Příklad: Kopírování dat z Azure Blob Storage tooAzure Data Lake Store
Hello ukázkový kód v této části uvádí:

* Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Propojené služby typu [AzureDataLakeStore](#linked-service-properties).
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [AzureDataLakeStoreSink](#copy-activity-properties).

Hello příklady ukazují, jak časové řady dat z Azure Blob Storage je zkopírovat tooData Lake Store každou hodinu. 

**Propojená služba Azure Storage**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Propojená služba Azure Data Lake Store**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.
>

**Vstupní datová sada Azure Blob**

V následujícím příkladu hello, data je převzata z nového objektu blob každou hodinu (`"frequency": "Hour", "interval": 1`). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá hello rok, měsíc a den část hello počáteční čas. Název souboru Hello používá hodinu hello část hello počáteční čas. Hello `"external": true` nastavení informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Data Lake Store výstupní datovou sadu**

Následující příklad kopie dat tooData Lake Store Hello. Zkopíruje nová data Lake Store tooData každou hodinu.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


**Aktivita kopírování v kanálu s blob zdroj a jímka Data Lake Store**

V následujícím příkladu hello, hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse hello vstupní a výstupní datové sady. Aktivita kopírování Hello je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello `source` je typ nastaven příliš`BlobSource`a hello `sink` je typ nastaven příliš`AzureDataLakeStoreSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
        ]
    }
}
```

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a>Příklad: Kopírování dat z Azure Data Lake Store tooan objektů blob v Azure
Hello ukázkový kód v této části uvádí:

* Propojené služby typu [AzureDataLakeStore](#linked-service-properties).
* Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [AzureDataLakeStoreSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Kód Hello kopíruje data časové řady z Data Lake Store tooan objektů blob v Azure každou hodinu. 

**Propojená služba Azure Data Lake Store**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.
>

**Propojená služba Azure Storage**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Vstupní datové sady Azure Data Lake**

V tomto příkladu nastavení `"external"` příliš`true` informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
              "interval": 1
        },
        "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
        }
      }
}
```
**Výstupní datová sada Azure Blob**

V následujícím příkladu hello, data se zapisují nový objekt blob tooa každou hodinu (`"frequency": "Hour", "interval": 1`). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá hello rok, měsíc, den a čas část hello počáteční čas.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Aktivita kopírování v kanálu s na Azure Data Lake Store zdroj a jímka objektů blob**

V následujícím příkladu hello, hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse hello vstupní a výstupní datové sady. Aktivita kopírování Hello je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello `source` je typ nastaven příliš`AzureDataLakeStoreSource`a hello `sink` je typ nastaven příliš`BlobSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
         ]
    }
}
```

V definici aktivity kopírování hello můžete namapovat sloupce z toocolumns datové sady zdroje hello v datové sadě podřízený hello. Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Výkon a ladění
toolearn hello faktory, které ovlivňují výkon aktivity kopírování a jak toooptimize, najdete v části hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) článku.
