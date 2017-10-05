---
title: "Připojení SSL pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Informace pro konfiguraci Azure databáze MySQL a přidružené aplikace odpovídajícím způsobem používat připojení SSL"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="dfa43-103">Připojení SSL ve službě Azure Database pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="dfa43-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="dfa43-104">Azure databáze pro databázi MySQL podporuje připojení databázový server pro klientské aplikace pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="dfa43-104">Azure Database for MySQL supports connecting your database server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="dfa43-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfa43-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="dfa43-106">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="dfa43-106">Default settings</span></span>
<span data-ttu-id="dfa43-107">Ve výchozím nastavení měla by se nakonfigurovat službu databáze vyžadovat připojení SSL při připojování k MySQL.</span><span class="sxs-lookup"><span data-stu-id="dfa43-107">By default, the database service should be configured to require SSL connections when connecting to MySQL.</span></span>  <span data-ttu-id="dfa43-108">Je doporučeno, nezakazujte možností protokolu SSL, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="dfa43-108">It is recommended avoid disabling the SSL option whenever possible.</span></span> 

<span data-ttu-id="dfa43-109">Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím portálu Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="dfa43-109">When provisioning a new Azure Database for MySQL server through the Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="dfa43-110">Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" v rámci vašeho serveru na portálu Azure, zahrnout požadované parametry pro běžné jazyky pro připojení k databázovému serveru pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="dfa43-110">Likewise, connection strings that are pre-defined in the "Connection Strings" settings under your server in the Azure portal include the required parameters for common languages to connect to your database server using SSL.</span></span> <span data-ttu-id="dfa43-111">Parametr SSL se liší v závislosti na konektoru, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.</span><span class="sxs-lookup"><span data-stu-id="dfa43-111">The SSL parameter varies based on the connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="dfa43-112">Informace o tom, které chcete povolit nebo zakázat připojení SSL při vývoji aplikací, naleznete v [postup konfigurace protokolu SSL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="dfa43-112">To learn how to enable or disable SSL connection when developing application, please refer to [How to configure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfa43-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfa43-113">Next steps</span></span>
[<span data-ttu-id="dfa43-114">Knihovny připojení pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="dfa43-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
