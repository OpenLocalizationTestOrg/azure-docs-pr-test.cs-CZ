---
title: "aaaCommon FabricClient výjimky vydané | Microsoft Docs"
description: "Popisuje hello běžné výjimek a chyb, k nimž může být vyvolána hello rozhraní API FabricClient při provádění operací správy aplikací a clusteru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 55bb556b25150524ebc28756eb1bd3e91dc37853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-exceptions-and-errors-when-working-with-hello-fabricclient-apis"></a>Běžné výjimek a chyb při práci s hello FabricClient rozhraní API
Hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) rozhraní API umožňují cluster a aplikace správci tooperform úlohy správy na aplikace, služby nebo clusteru Service Fabric. Například nasazení aplikace, upgrade a odebrání, kontrola stavu hello cluster nebo testování služby. Vývojáři aplikací a Správce clusteru můžete použít hello rozhraní API FabricClient toodevelop nástroje pro správu clusteru Service Fabric hello a aplikace.

Existuje mnoho různých typů operací, které je možné provést pomocí FabricClient.  Každá metoda může vyvolat výjimky chyby kvůli tooincorrect vstup, chyby za běhu nebo problémů s přechodnou infrastrukturou.  V tématu hello API referenční dokumentaci toofind výjimek, které jsou vyvolány pomocí konkrétní metody. Existují některé výjimky, ale to může být vyvolána mnoho různých [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) rozhraní API. Hello následující tabulka uvádí hello výjimky, které jsou společné pro hello FabricClient rozhraní API.

| Výjimka | Při vyvolání |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |Hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objekt je v uzavřeném stavu. Odstranění hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) používáte a vytvořit novou instanci objektu [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektu. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |Vypršel časový limit operace Hello. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) je vrácena, pokud operace hello trvá déle než jako MaxOperationTimeout toocomplete. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |Kontrola přístupu Hello hello operace se nezdařila. E_ACCESSDENIED je vrácen. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |Při provádění operace hello došlo k chybě za běhu. Libovolná z metod FabricClient hello potenciálně throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), hello [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) vlastnost udává hello přesné příčině hello výjimky. Kódy chyb jsou definovány v hello [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) výčtu. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |Hello operace se nezdařila z důvodu tooa přechodný chybový stav určitého druhu. Například operace může selhat, protože kvorum repliky není dočasně dostupná. Přechodný výjimky odpovídají toofailed operace, které můžete zkusit znovu. |

Některé běžné [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) chyb, které mohou být vráceny v [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Chyba | Podmínka |
| --- |:--- |
| CommunicationError |Komunikační chyba způsobila hello operaci toofail, operace hello opakování. |
| InvalidCredentialType |Typ přihlašovacích údajů Hello je neplatný. |
| InvalidX509FindType |Hello X509FindType je neplatný. |
| InvalidX509StoreLocation |umístění úložiště Hello X509 je neplatný. |
| InvalidX509StoreName |Název úložiště Hello X509 je neplatný. |
| InvalidX509Thumbprint |Hello X509 řetězec kryptografický otisk certifikátu je neplatný. |
| InvalidProtectionLevel |úroveň ochrany Hello je neplatný. |
| InvalidX509Store |úložiště certifikátů Hello X509 nelze otevřít. |
| InvalidSubjectName |Název subjektu Hello je neplatný. |
| InvalidAllowedCommonNameList |Formát Hello řetězce seznamu běžný název je neplatný. To by měl být čárkami oddělený seznam. |

