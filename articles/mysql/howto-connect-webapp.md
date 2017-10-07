---
title: "aaaConnect stávající služby Azure App Service tooAzure databáze pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak tooproperly připojit stávající služby Azure App Service tooAzure databáze pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6d5b16f316e186d665370adcd8b7c7bb38c8d51a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a>Připojit existující tooAzure Azure App Service databáze MySQL serveru
Tento dokument popisuje, jak tooconnect stávající služby Azure App Service tooyour Azure databáze pro server databáze MySQL.

## <a name="before-you-begin"></a>Než začnete
Přihlaste se toohello [portál Azure](https://portal.azure.com). Vytvoření databáze Azure pro server databáze MySQL. Podrobnosti najdete příliš[jak toocreate Azure databáze MySQL serveru z portálu](quickstart-create-mysql-server-database-using-azure-portal.md) nebo [jak toocreate Azure databáze MySQL serveru pomocí rozhraní příkazového řádku](quickstart-create-mysql-server-database-using-azure-cli.md).

Nyní nejsou k dispozici dva přístup řešení tooenable ze Azure App Service tooan Azure Database pro databázi MySQL. Obě řešení zahrnují nastavení pravidla brány firewall na úrovni serveru.

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a>Řešení 1 – Vytvoření tooallow pravidlo brány firewall všechny IP adresy
Databáze pro databázi MySQL Azure poskytuje přístup k zabezpečení pomocí brány firewall tooprotect vaše data. Při připojení z Azure App Service tooAzure databáze pro server databáze MySQL, mějte na paměti této hello odchozí jsou ve své podstatě dynamické IP adresy služby App Service. 

tooensure hello dostupnost služby Azure App Service, doporučujeme používat toto řešení tooallow všechny IP adresy.

> [!NOTE]
> Společnost Microsoft pracuje pro dlouhodobé tooavoid řešení, aby vám umožnil všechny IP adresy pro služby Azure tooconnect tooAzure databáze MySQL.

1. V okně serveru MySQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello Azure Database pro databázi MySQL.

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Zadejte **název pravidla**, **počáteční IP adresa**, a **KONCOVÁ IP adresa**. Potom klikněte na **Uložit**.
   - Název pravidla: Povolit – všechny-IP adresy
   - Spustit IP: 0.0.0.0
   - Ukončení IP: 255.255.255.255

   ![Portál Azure – přidání všechny IP adresy](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a>Řešení 2 – Vytvoření brány firewall povolit pravidlo tooexplicitly odchozí IP adresy
Můžete explicitně přidat, že všechny hello odchozí IP adresy služby Azure App Service.

1. V okně Vlastnosti služby aplikace hello, zobrazit vaše **ODCHOZÍ IP adresu**.

   ![Portál Azure – zobrazení odchozí IP adresy](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. V okně zabezpečení hello připojení MySQL přidáte odchozí IP adres po jednom.

   ![Portál Azure – přidání explicitní IP adresy](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Mějte na paměti, příliš**Uložit** vašich pravidlech brány firewall.

I když hello služby Azure App service pokusí tookeep IP adresy konstanta v čase, jsou případy, kde může změnit hello IP adresy. Například když hello aplikace recykluje nebo dojde k operaci zvětšení nebo při přidávání nových počítačů v Azure místní data centra tooincrease hello kapacity. Při změně adres hello IP, aplikace hello se může setkat s výpadku v případě hello už můžete připojit toohello MySQL server. Zvažte tato potenciální při výběru jednoho hello předcházející řešení.

## <a name="ssl-configuration"></a>Konfigurace protokolu SSL
Azure databáze MySQL má ve výchozím nastavení povolen protokol SSL. Pokud vaše aplikace nepoužívá SSL tooconnect toohello databáze, musíte toodisable SSL na serveru databáze MySQL. Podrobnosti o tom tooconfigure SSL, najdete v tématu [pomocí protokolu SSL s Azure Database pro databázi MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Další kroky
Další informace o připojovacích řetězcích najdete příliš[připojovací řetězce](howto-connection-string.md).
