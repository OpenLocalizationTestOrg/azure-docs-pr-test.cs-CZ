---
title: "aaaConfigure připojovací řetězec pro Azure Storage | Microsoft Docs"
description: "Nakonfigurujte připojovací řetězec pro účet úložiště Azure. Připojovací řetězec obsahuje informace o hello potřeby tooauthenticate přístup k účtu úložiště tooa z vaší aplikace za běhu."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: marsma
ms.openlocfilehash: 80c38a6f8f0d4f06b99e7c487647b984e01d1772
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-storage-connection-strings"></a>Nakonfigurování připojovacích řetězců Azure Storage

Připojovací řetězec obsahuje hello ověřovací informace požadované pro vaše aplikace tooaccess data v účtu Azure Storage za běhu. Můžete nakonfigurovat připojovací řetězce pro:

* Připojte toohello emulátoru úložiště Azure.
* Přístup k účtu úložiště v Azure.
* Přístup k zadané prostředky v Azure pomocí sdíleného přístupového podpisu (SAS).

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Ukládání připojovací řetězec
Aplikace musí tooaccess hello připojovací řetězec na modulu runtime tooauthenticate požadavkům tooAzure úložiště. Máte několik možností pro ukládání připojovací řetězec:

* Je spuštěna aplikace na ploše hello nebo na zařízení můžete ukládat hello připojovací řetězec **app.config** nebo **web.config** souboru. Přidat hello připojovací řetězec toohello **AppSettings** část v těchto souborech.
* Aplikace běžící v cloudové službě Azure můžete ukládat hello připojovací řetězec v hello [konfigurační soubor schématu (.cscfg) Azure služby](https://msdn.microsoft.com/library/ee758710.aspx). Přidat hello připojovací řetězec toohello **ConfigurationSettings** části konfigurační soubor služby hello.
* Můžete vytvořit připojovací řetězec přímo v kódu. Doporučujeme však připojovací řetězec uložit v konfiguračním souboru ve většině scénářů.

Ukládání připojovací řetězec v konfiguračním souboru umožňuje snadno tooupdate hello připojovací řetězec tooswitch mezi hello emulátor úložiště a účet úložiště Azure v cloudu hello. Potřebujete jenom tooedit hello připojovací řetězec toopoint tooyour cílovém prostředí.

Můžete použít hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess váš připojovací řetězec za běhu bez ohledu na to, kde je aplikace spuštěna.

## <a name="create-a-connection-string-for-hello-storage-emulator"></a>Vytvořit připojovací řetězec pro emulátor úložiště hello
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Další informace o emulátoru úložiště hello najdete v tématu [používejte hello emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Vytvoření připojovacího řetězce pro účet úložiště Azure
toocreate připojovací řetězec pro váš účet úložiště Azure, použijte následující hello formátu. Označuje, zda chcete účet úložiště toohello tooconnect prostřednictvím protokolu HTTPS (doporučeno) nebo protokolu HTTP, nahraďte `myAccountName` hello název účtu úložiště a nahraďte `myAccountKey` vaším přístupovým klíčem účet:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Například připojovací řetězec může vypadat podobně jako:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

I když Azure Storage podporuje protokoly HTTP a HTTPS v připojovacím řetězci, *HTTPS se důrazně doporučuje*.

> [!TIP]
> Připojovací řetězce účtu úložiště můžete najít v hello [portál Azure](https://portal.azure.com). Přejděte příliš**nastavení** > **přístupové klíče** v účtu úložiště nabídky okno toosee připojovací řetězce pro obě primární a sekundární přístupové klíče.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Vytvoření připojovacího řetězce pomocí sdíleného přístupového podpisu
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Vytvoření připojovacího řetězce pro koncový bod explicitní úložiště
Koncové body explicitní služby můžete zadat v připojovací řetězec místo použití výchozí hello koncové body. toocreate připojovací řetězec, který určuje explicitní koncový bod, zadejte hello koncový bod dokončení služby pro jednotlivé služby včetně hello specifikace protokolu (HTTPS (doporučeno) nebo HTTP), v hello následující formát:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Jeden scénář, kde byste mohli toospecify explicitní koncový bod je, když jste změnili vaší tooa koncový bod úložiště objektů Blob [vlastní domény](storage-custom-domain-name.md). V takovém případě můžete zadat svůj vlastní koncový bod pro úložiště objektů Blob v připojovacím řetězci. Volitelně můžete zadat hello výchozí koncové body pro hello jiných služeb Pokud vaše aplikace používá je.

Tady je příklad připojovacího řetězce, který určuje explicitní koncový bod pro hello služby objektů Blob:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Tento příklad určuje explicitní koncových bodů pro všechny služby, včetně vlastní doménu pro hello služby objektů Blob:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

jsou použité tooconstruct hello žádost služby úložiště toohello identifikátory URI Hello koncový bod hodnoty v připojovacím řetězci a stanovují hello formu všechny identifikátory URI, který se vrátil kód tooyour.

Pokud jste namapovat vlastní doménu tooa koncový bod úložiště a pak už nebudete moct toouse vynechejte tohoto koncového bodu z připojovacího řetězce, že připojovací řetězec tooaccess data v této službě z vašeho kódu.

> [!IMPORTANT]
> Hodnoty koncového bodu služby v připojovací řetězce musí být ve správném formátu identifikátory URI, včetně `https://` (doporučeno) nebo `http://`. Protože Azure Storage ještě nepodporuje protokol HTTPS pro vlastní domény, můžete *musí* zadejte `http://` pro libovolný koncový bod identifikátor URI, který ukazuje tooa vlastní doménu.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Vytvoření připojovacího řetězce s příponou koncový bod
toocreate připojovací řetězec pro službu úložiště v oblasti nebo instancí s přípon jinému koncovému bodu, například pro Azure China nebo Azure Government, hello použijte následující formát řetězce připojení. Označuje, zda chcete účet úložiště toohello tooconnect prostřednictvím protokolu HTTPS (doporučeno) nebo protokolu HTTP, nahraďte `myAccountName` nahradil hello název svého účtu úložiště, `myAccountKey` se přístupový klíč účtu a nahraďte `mySuffix` s hello identifikátor URI Přípona:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Tady je příklad řetězce připojení pro služby úložiště v Číně Azure:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Analýza připojovacího řetězce
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Další kroky
* [Používejte hello emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md)
* [Azure průzkumníci úložiště](storage-explorers.md)
* [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md)

