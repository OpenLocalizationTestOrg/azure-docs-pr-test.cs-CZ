---
title: "aaaManage Azure Data Lake Analytics pomocí sady Azure SDK pro Node.js | Microsoft Docs"
description: "Zjistěte, jak toomanage účtů Data Lake Analytics, datových zdrojů, úloh a uživatelé pomocí sady Azure SDK pro Node.js"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 9de1bcf4-b15b-4d0b-9284-8889ecf0c438
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 07acd058bf252af2fc98c4cfe87a135e0b79900f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Správa Azure Data Lake Analytics pomocí sady Azure SDK pro Node.js
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure SDK pro Node.js Hello lze použít pro správu účtů Azure Data Lake Analytics, úlohy a katalogů. téma správy toosee pomocí jiných nástrojů, klikněte na výběr karty hello výše.

Aktuálně podporuje:

* **Verze Node.js: 0.10.0 nebo vyšší**
* **Verze rozhraní REST API pro Účet: 2015-10-01-preview**
* **Verze rozhraní REST API pro Katalog: 2015-10-01-preview**
* **Verze rozhraní REST API pro Úlohu: 2016-03-20 preview**

## <a name="features"></a>Funkce
* Správa účtů: vytvoření, získání, seznam, aktualizace a odstranění.
* Správa úloh: odeslání, získání, seznam a zrušení.
* Správa katalogů: získání a seznam.

## <a name="how-tooinstall"></a>Jak tooInstall
```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Ověření pomocí služby Azure Active Directory
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-hello-data-lake-analytics-client"></a>Vytvoření klienta Data Lake Analytics hello
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created toocreate
// a Data Lake Analytics account. See hello Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object toocreate
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference toohello actual request and response, so you can see what was sent and received on hello wire.
      hello structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'hello response body if any',
        request: reference tooa stripped version of http request
        response: reference tooa stripped version of hello response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Získání seznamu úloh
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-hello-data-lake-analytics-catalog"></a>Získání seznamu databází v hello katalogu Data Lake Analytics
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Viz také
* [Microsoft Azure SDK pro Node.js](https://github.com/azure/azure-sdk-for-node)
* [Microsoft Azure SDK pro Node.js – správa Data Lake Storu](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)

