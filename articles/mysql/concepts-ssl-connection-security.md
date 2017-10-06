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
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="2c062-103">Připojení SSL ve službě Azure Database pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="2c062-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="2c062-104">Azure databáze pro databázi MySQL podporuje připojení aplikace tooclient databáze serveru pomocí Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="2c062-104">Azure Database for MySQL supports connecting your database server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="2c062-105">Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c062-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="2c062-106">Výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="2c062-106">Default settings</span></span>
<span data-ttu-id="2c062-107">Ve výchozím nastavení služba hello databáze musí být nakonfigurované toorequire připojení SSL při připojování tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="2c062-107">By default, hello database service should be configured toorequire SSL connections when connecting tooMySQL.</span></span>  <span data-ttu-id="2c062-108">Je doporučeno, nezakazujte hello možností protokolu SSL, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="2c062-108">It is recommended avoid disabling hello SSL option whenever possible.</span></span> 

<span data-ttu-id="2c062-109">Při zřizování nové databáze Azure pro server databáze MySQL prostřednictvím hello portál Azure a rozhraní příkazového řádku, je ve výchozím nastavení povolena vynucení připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="2c062-109">When provisioning a new Azure Database for MySQL server through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="2c062-110">Připojovací řetězce, které jsou předem definované v nastavení "Připojovací řetězce" hello v rámci vašeho serveru v hello portál Azure, zahrnují hello požadované parametry pro běžné jazyky tooconnect tooyour databáze serveru pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="2c062-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="2c062-111">Hello parametr SSL se liší podle hello konektor, například "ssl = true" nebo "sslmode = vyžadují" nebo "sslmode = požadované" a dalších odlišností.</span><span class="sxs-lookup"><span data-stu-id="2c062-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="2c062-112">jak tooenable nebo zakázat připojení SSL při vývoji aplikací, naleznete v příliš toolearn[jak tooconfigure SSL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2c062-112">toolearn how tooenable or disable SSL connection when developing application, please refer too[How tooconfigure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c062-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c062-113">Next steps</span></span>
[<span data-ttu-id="2c062-114">Knihovny připojení pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="2c062-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
