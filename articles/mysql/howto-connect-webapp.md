---
title: "Existující Azure App Service připojit k databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Pokyny, jak správně existující Azure App Service připojit k databázi Azure pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 0028b0c918b573f6884e6f63fa82dbe23079882a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Existující Azure App Service připojit k databázi Azure pro server databáze MySQL
Toto téma vysvětluje, jak se připojit k vaší databázi Azure pro server databáze MySQL existující Azure App Service.

## <a name="before-you-begin"></a>Než začnete
Přihlaste se k portálu [Azure Portal](https://portal.azure.com). Vytvoření databáze Azure pro server databáze MySQL. Podrobnosti najdete v části [vytvoření Azure databáze MySQL serveru z portálu](quickstart-create-mysql-server-database-using-azure-portal.md) nebo [vytvoření Azure databáze MySQL serveru pomocí rozhraní příkazového řádku](quickstart-create-mysql-server-database-using-azure-cli.md).

Aktuálně existují dvě řešení pro povolení přístupu z aplikační službu Azure do Azure databáze pro databázi MySQL. Obě řešení zahrnují nastavení pravidla brány firewall na úrovni serveru.

## <a name="solution-1---create-a-firewall-rule-to-allow-all-ips"></a>Řešení 1 – Vytvoření pravidla brány firewall umožňující všechny IP adresy
Azure databáze pro databázi MySQL poskytuje přístup k zabezpečení pomocí brány firewall chránit vaše data. Při připojení ze Azure App Service k databázi Azure pro server databáze MySQL, mějte na paměti, že jsou dynamické ve své podstatě odchozí IP adresy App Service. 

Aby dostupnost služby Azure App Service, doporučujeme používat toto řešení umožňuje všechny IP adresy.

> [!NOTE]
> Společnost Microsoft pracuje na dlouhodobé řešení, aby se zabránilo povolení všechny IP adresy pro Azure services pro připojení k databázi Azure pro databázi MySQL.

1. V okně serveru MySQL, v části nastavení klikněte na **zabezpečení připojení** otevřete okno zabezpečení připojení pro databázi Azure pro databázi MySQL.

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Zadejte **název pravidla**, **počáteční IP**, a **KONCOVÁ IP adresa**a potom klikněte na **Uložit**.
   - Název pravidla: Povolit – všechny-IP adresy
   - Spustit IP: 0.0.0.0
   - Ukončení IP: 255.255.255.255

   ![Portál Azure – přidání všechny IP adresy](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Řešení 2 – vytvořte pravidlo brány firewall pro výslovně povolit odchozí IP adresy
Můžete explicitně přidat všechny odchozí IP adresy služby Azure App Service.

1. V okně Vlastnosti služby aplikace zobrazit vaše **ODCHOZÍ IP adresu**.

   ![Portál Azure – zobrazení odchozí IP adresy](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. V okně zabezpečení připojení MySQL přidáte odchozí IP adres po jednom.

   ![Portál Azure – přidání explicitní IP adresy](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Nezapomeňte **Uložit** vašich pravidlech brány firewall.

I když Azure App service se pokusí zachovat konstantní IP adresy v čase, jsou případy, kdy může změnit IP adresy. Například to může dojít, když dojde k aplikace recykluje nebo operace škálování, nebo při přidávání nové počítače v Azure místní data centra navýšení kapacity. Při změně adres IP, může aplikace prostředí výpadku v případě, že už se může připojit k serveru databáze MySQL. Mějte na tento zvážení při výběru jednoho z uvedených postupů.

## <a name="ssl-configuration"></a>Konfigurace protokolu SSL
Azure databáze MySQL, má protokol SSL ve výchozím nastavení povolené. Pokud vaše aplikace není pomocí protokolu SSL pro připojení k databázi, budete muset zakázat protokol SSL na serveru databáze MySQL. Podrobnosti o tom, jak konfigurace protokolu SSL najdete v tématu [pomocí protokolu SSL s Azure Database pro databázi MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Další kroky
Další informace o připojovacích řetězcích najdete v části [připojovací řetězce](howto-connection-string.md).
