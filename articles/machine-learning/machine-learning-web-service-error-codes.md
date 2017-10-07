---
title: "kódy chyb aaaAzure Machine Learning REST API | Microsoft Docs"
description: "Tyto kódy chyb mohou být vráceny při operace na webové služby Azure Machine Learning."
keywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/16/2016
ms.author: garye
ms.openlocfilehash: 9495c8ef16e684d3c8978bcd11747cf95227b091
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-rest-api-error-codes"></a>Strojového učení kódy chyb rozhraní API REST
 
Hello následující kódy chyb mohou být vráceny při operace na webové služby Azure Machine Learning.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (kód stavu HTTP 400)
 
Zadán neplatný argument.
 
Tato třída chyb znamená, že argument zadaný někde byl neplatný. To může být přihlašovacích údajů nebo umístění úložiště Azure toosomething předán toohello webové služby. Podívejte se prosím na pole kód"chyby" hello v toodiagnose část "Podrobnosti" hello, které konkrétní argument byl neplatný.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| BadParameterValue | Zadaná hodnota parametru Hello nevyhovuje hello pravidlo parametr v parametru hello |
| BadSubscriptionId | předplatné Hello Id, které je použité tooscore není hello jedna nachází v prostředku hello |
| BadVersionCall | Během volání rozhraní API hello byl předán parametr neplatné verze: {0}. Zkontrolujte hello API stránka pro předávání hello správnou verzi nápovědy a zkuste to znovu. |
| BatchJobInputsNotSpecified | Hello následující požadované input(s) nebyly zadány s žádostí hello: {0}. Přesvědčte se, zda je zadán veškerá vstupní data a zkuste to znovu. |
| BatchJobInputsTooManySpecified | Hello žádost zadat další vstupy než definované ve službě hello. Seznam přijatý input(s): {0}. Přesvědčte se, zda je správně zadán veškerá vstupní data a zkuste to znovu. |
| BlobNameTooLong | Cestu k úložišti objektů blob v Azure zadaná pro výstup diagnostiky je příliš dlouhá: {0}. Zkrácení hello cesty a zkuste to znovu. |
| BlobNotFound | Azure blob - zadat nelze tooaccess hello {0}.  Chybová zpráva Azure: {1}. |
| ContainerIsEmpty | Nebyly poskytnuty žádné název kontejneru úložiště Azure. Zadejte název platné kontejneru a zkuste to znovu. |
| ContainerSegmentInvalid | Název kontejneru neplatný. Zadejte název platné kontejneru a zkuste to znovu. |
| ContainerValidationFailed | Objekt BLOB kontejneru ověření se nezdařilo s touto chybou: {0}. |
| DataTypeNotSupported | Nepodporovaný datový typ zadaný. Zadejte platný datový typ (typy) a zkuste to znovu. |
| DuplicateInputInBatchCall | Dávková žádost Hello je neplatný. Nelze zadat jedním i několika vstup na hello stejný čas. Odeberte jeden z těchto položek z požadavku hello a zkuste to znovu. |
| ExpiryTimeInThePast | Zadaný čas vypršení platnosti je v minulosti hello: {0}. Zadejte budoucí vypršení platnosti čas v UTC a zkuste to znovu. toonever vyprší, nastavte tooNULL čas vypršení platnosti. |
| IncompleteSettings | Nastavení diagnostiky jsou neúplné. |
| InputBlobRelativeLocationInvalid | K dispozici žádný název objektu blob úložiště Azure. Zadejte platný objekt blob název a akci opakujte. |
| InvalidBlob | Specifikace neplatný objekt blob pro objekt blob: {0}. Ověřte, že připojovací řetězec / relativní cestu nebo specifikace token SAS je správný a zkuste to znovu. |
| InvalidBlobConnectionString | Hello připojovacího řetězce zadaného pro jeden z objektů BLOB vstupu a výstupu hello neplatné: {0}. To opravte a zkuste to znovu. |
| InvalidBlobExtension | Hello odkaz na objekt blob: {0} má neplatný nebo chybějící souboru rozšíření. Podporovaný přípony souborů pro tento typ výstupu jsou: "{1}". |
| InvalidInputNames | Neplatná služba vstupní názvy určený v požadavku hello: {0}. Mapování hello vstupní data toohello správné služby vstupy a zkuste to znovu. |
| InvalidOutputOverrideName | Název přepsání neplatný výstup: {0}. Služba Hello nemá do uzlu výstup s tímto názvem. Předejte prosím toooverride název uzlu správné výstup (platí malá a velká písmena). |
| InvalidQueryParameter | Neplatný parametr dotazu '{0}'. {1} |
| MissingInputBlobInformation | Chybí informace o objektu blob úložiště Azure. Zadejte platný připojovací řetězec a relativní cestu nebo identifikátor URI a zkuste to znovu. |
| MissingJobId | Zadané Id žádná úloha. Úlohu Id je vrácena, pokud úloha byla odeslána pro hello poprvé. Ověření úlohy hello Id je správný a zkuste to znovu. |
| MissingKeys | Zadáno žádné klíče nebo jeden z primární nebo sekundární klíč není k dispozici. |
| MissingModelPackage | Žádné Id balíčku modelu nebo zadaný balíček modelu. Zadejte platný model balíček Id nebo modelu balíčku a zkuste to znovu. |
| MissingOutputOverrideSpecification | žádost o Hello chybí hello blob specifikace pro výstup přepsání {0}. Zadejte platný objekt blob umístění hello požadavku, nebo odeberte specifikaci výstup hello, pokud se požaduje přepsání není umístění. |
| MissingRequestInput | Webová služba Hello očekává vstup, ale nebyl poskytnut žádný vstup. Zajistěte platné vstupní hodnoty jsou uvedeny na hello zveřejněna základě vstupních portů ve hello model a zkuste to znovu. |
| MissingRequiredGlobalParameters | Ne všechny požadované parametry webové služby poskytuje. Zkontrolujte parametry hello očekávání pro hello modulu nebo modulech jsou správné a zkuste to znovu. |
| MissingRequiredOutputOverrides | Při volání metody koncový bod šifrované služby, které je povinné toopass ve výstupu přepsání pro všechny služby hello výstupy. Chybí přepsání v tuto chvíli pro tyto výstupy: {0} |
| MissingWebServiceGroupId | Zadané Id žádná skupina webové služby. Zadejte Id platný webové služby skupiny a zkuste to znovu. |
| MissingWebServiceId | Zadané Id žádné webové služby. Zadejte platný webovou službu Id a zkuste to znovu. |
| MissingWebServicePackage | Žádný balíček služby web poskytuje. Zadejte balíček platný webové služby a zkuste to znovu. |
| MissingWorkspaceId | Žádné pracovní prostor pro zadané Id. Zadejte platné Id pracovního prostoru a zkuste to znovu. |
| ModelConfigurationInvalid | Neplatný model konfigurace v balíčku modelu hello. Zkontrolujte konfiguraci modelu hello obsahuje definici koncové výstup, – std chyba koncovému bodu, a – std koncový bod a zkuste to znovu. |
| ModelPackageIdInvalid | Neplatný model balíček ID. Ověřte správnost tohoto hello model Id balíčku a zkuste to znovu. |
| RequestBodyInvalid | Žádné poskytuje textu žádosti nebo Chyba při deserializaci textu hello žádosti. |
| RequestIsEmpty | Není zadaný žádný požadavek. Zadejte platnou žádost a zkuste to znovu. |
| UnexpectedParameter | Neočekávané parametry zadané. Ověřte všechny názvy parametrů jsou správně zadané, pouze očekávané parametrů a zkuste to znovu. |
| Neznámé chyby | Došlo k neznámé chybě. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | Nelze změnit požadavky souběžných požadavků pro webovou službu {0}. |
| WebServiceIdInvalid | Zadané id neplatný webové služby. Id webové služby musí být platný identifikátor guid. |
| WebServiceTooManyConcurrentRequestRequirement | Nelze nastavit počtu souběžných požadavků požadavek toomore než {0}. |
| WebServiceTypeInvalid | Zadaný typ neplatný webové služby. Ověřte, zda je správný typ hello platný webové služby a akci opakujte. Typy platný webové služby: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (kód stavu HTTP 400)
 
Zadán neplatný uživatel argument.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| InputMismatchError | Vstupních dat neodpovídá schématu vstupní port. |
| InputParseError | Analýza vstupu vektoru se nezdařilo.  Ověřte, zda má vstupní vektor hello hello správný počet sloupců a datové typy.  Další podrobnosti: {0}. |
| MissingRequiredGlobalParameters | Chybí parametry očekávanou hello webové služby. Ověřte, zda jsou všechny parametry hello požadované očekává webovou službou hello správné a zkuste to znovu. |
| UnexpectedParameter | Ověřte, že pouze hello požadované jsou předány parametry očekávanou hello webové služby a akci opakujte. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (kód stavu HTTP 400)
 
požadavek Hello je v aktuálním kontextu hello.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| CannotStartJob | Hello úlohu nelze spustit, protože je ve stavu {0}. |
| IncompatibleModel | Hello model je kompatibilní s verzí hello požadavku. Hello požadavek verze podporuje pouze jeden element datatable výstup modely. |
| MultipleInputsNotAllowed | Hello model nepovoluje více vstupů. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (kód stavu HTTP 400)
 
Spouštění modulu došlo k chybě interní knihovna.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (kód stavu HTTP 400)
 
Spouštění modulu došlo k chybě.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (kód stavu HTTP 400)
 
Balíček neplatný webové služby. Ověřte správnost hello webové služby balíček zadaný a zkuste to znovu.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| FormatError | balíček Hello webové služby je poškozený. Podrobnosti: {0} |
| RuntimesError | Hello webové služby balíček grafu je neplatný. Podrobnosti: {0} |
| ValidationError | Hello webové služby balíček grafu je neplatný. Podrobnosti: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Neoprávněné (kód stavu HTTP 401)
 
Žádost je neoprávněným tooaccess prostředků.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| AdminRequestUnauthorized | Neautorizováno |
| ManagementRequestUnauthorized | Neautorizováno |
| ScoreRequestUnauthorized | Zadané neplatné přihlašovací údaje. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (kód stavu HTTP 404)
 
Prostředek se nenašel.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| ModelPackageNotFound | Modelu balíček nebyl nalezen. Ověřte hello modelu balíček Id je správný a zkuste to znovu. |
| WebServiceIdNotFoundInWorkspace | Webová služba v rámci tohoto pracovního prostoru nebyl nalezen. Došlo k neshodě mezi hello webServiceId a hello ID pracovního prostoru. Ověřte hello webové služby je součástí hello prostoru a zkuste to znovu. |
| WebServiceNotFound | Webové služby nebyl nalezen. Ověřte hello webová služba Id je správný a zkuste to znovu. |
| WorkspaceNotFound | Pracovní prostor nebyl nalezen. Ověřte hello prostoru Id je správný a zkuste to znovu. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (kód stavu HTTP 408)
 
Hello operaci nelze dokončit v hello povolené čas.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| RequestCanceled | Požadavek byl zrušen klientem hello. |
| ScoreRequestTimeout | Vypršel časový limit provádění požadavku. |
 
## <a name="conflict-http-status-code-409"></a>Konflikt (kód stavu HTTP 409)
 
Prostředek již existuje.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Název parametru neplatný výstup. Zkuste použít hello metadata editor modulu toorename sloupce a zkuste to znovu. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (kód stavu HTTP 413)
 
Hello model překročil kvótu paměti hello přiřazené tooit.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| OutOfMemoryLimit | Hello model využívat více paměti, než byla přidělena pro ni. Maximální povolená velikost paměti pro hello model je {0} MB. Zkontrolujte prosím váš model problémů. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (kód stavu HTTP 500)
 
Spuštění došlo k vnitřní chybě.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | proces kontejneru Hello selhal s chybou systému |
| ContainerProcessTerminatedWithUnknownError | došlo k chybě procesu kontejneru Hello k neznámé chybě |
| ContainerValidationFailed | Objekt BLOB kontejneru ověření se nezdařilo s touto chybou: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Žádné argumenty. Ověřte, zda platnými argumenty jsou předávány a zkuste to znovu. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Id portu = {0} má nepodporovaný datový typ: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Generování swagger se nezdařila, podrobnosti: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| Neznámé chyby |  |
| UnknownJobStatusCode | Neznámá úloha kódem stavu {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, podrobnosti: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (kód stavu HTTP 500)
 
Spuštění došlo k vnitřní chybě. Systém nemá dostatek paměti. Zkuste to prosím znovu.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (kód stavu HTTP 500)
 
Neplatný model balíček. Ověřte správnost balíčku modelu hello zadaný a zkuste to znovu.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (kód stavu HTTP 500)
 
Balíček neplatný webové služby. Ověřte správnost zadané hello webového balíčku a zkuste to znovu.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| ModuleError | Hello webové služby balíček grafu je neplatný. Podrobnosti: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (kód stavu HTTP 503)
 
Hello požadavek nelze provést jako hello kontejnery není inicializován.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (kód stavu HTTP 503)
 
Služba je dočasně nedostupná.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| NoMoreResources | Pro požadavek k dispozici žádné prostředky. |
| RequestThrottled | Žádost byla omezena pro koncový bod {0}. Hello maximální souběžnosti pro koncový bod hello je {1}. |
| TooManyConcurrentRequests | Moc souběžných žádostí, které jsou odeslána. |
| TooManyHostsBeingInitialized | Příliš mnoho hostitelů během inicializace na hello stejný čas. Vezměte v úvahu omezení / opakování. |
| TooManyHostsBeingInitializedPerModel | Příliš mnoho hostitelů během inicializace na hello stejný čas. Vezměte v úvahu omezení / opakování. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (stavový kód HTTP 504)
 
Hello operaci nelze dokončit v hello povolené čas.
 
| Kód chyby | Zpráva uživatele |
| ---------- |--------------|
| BackendInitializationTimeout | Inicializace služby web Hello nebylo možné dokončit v hello povolené čas. |
| BackendScoreTimeout | Hello webové služby žádost o spuštění nebylo možné dokončit v hello povolené čas. |
 
