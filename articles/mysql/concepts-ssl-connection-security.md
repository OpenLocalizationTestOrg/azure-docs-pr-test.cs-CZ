---
title: "aaaSSL připojení pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Informace pro konfiguraci Azure databáze MySQL a přidružené aplikace tooproperly pomocí připojení SSL"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6fca7c88fc0f1fd6058d68fcff90fd409abd97a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Připojení SSL ve službě Azure Database pro databázi MySQL
Azure databáze pro databázi MySQL podporuje připojení aplikace tooclient databáze serveru pomocí Secure Sockets Layer (SSL). Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.

## <a name="default-settings"></a>Výchozí nastavení
Ve výchozím nastavení služba hello databáze musí být nakonfigurované toorequire připojení SSL při připojování tooMySQL.  Je doporučeno, nezakazujte hello možností protokolu SSL, kdykoli je to možné. 

Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím hello portál Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL. 

Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" hello v rámci vašeho serveru v hello portál Azure, zahrnují hello požadované parametry pro běžné jazyky tooconnect tooyour databáze serveru pomocí protokolu SSL. Hello parametr SSL se liší podle hello konektor, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.

jak tooenable nebo zakázat připojení SSL při vývoji aplikací, naleznete v příliš toolearn[jak tooconfigure SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Další kroky
[Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
