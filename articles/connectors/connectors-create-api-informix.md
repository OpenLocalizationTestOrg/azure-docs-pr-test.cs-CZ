---
title: "konektor Informix hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru Informix s parametry rozhraní REST API"
services: 
documentationcenter: 
author: gplarsen
manager: anneta
editor: 
tags: connectors
ms.assetid: ca2393f0-3073-4dc2-8438-747f5bc59689
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: 7a163e2ebf00fa3109b93e34845d922c2174a48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-informix-connector"></a>Začínáme s konektorem Informix hello
Konektor Microsoft pro Informix připojí uložené v databázi IBM Informix tooresources Logic Apps. konektor Informix Hello zahrnuje server počítačů Microsoft klienta toocommunicate tooremote Informix přes síť TCP/IP. To zahrnuje cloudu databáze, například IBM Informix pro systém Windows spuštěn v Azure virtualizace a místní databáze pomocí hello místní data gateway. V tématu hello [podporované seznamu](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix platforem a verzí (v tomto tématu).

Hello konektor podporuje hello následujících databázové operace:

* Seznam tabulek databáze
* Pomocí vyberte jeden řádek pro čtení
* Číst všechny řádky pomocí vyberte
* Přidejte jeden řádek pomocí vložení
* Příkaz ALTER jeden řádek použití aktualizace
* Odeberte jeden řádek používání příkazu DELETE

Toto téma ukazuje, jak toouse hello konektoru logiku aplikace tooprocess databáze operace.

toolearn Další informace o Logic Apps, najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Dostupné akce
Tento konektor podporuje následující akce aplikace logiky hello:

* Getables
* GetRow
* Proces GetRows
* Insertrow –
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Seznam tabulek
Vytvoření aplikace logiky pro všechny operace se skládá z mnoho kroků, které se provádí prostřednictvím portálu Microsoft Azure hello.

V rámci aplikace logiky hello můžete přidat akci toolist tabulky v databázi Informix. Tato akce dá pokyn hello konektor tooprocess výpis Informix schématu, jako například `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `InformixgetTables`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**.  
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - Get tabulky (Preview)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. V hello **Informix - Get tabulky** konfigurace podokně, vyberte **políčko** tooenable **připojit prostřednictvím místní brána dat**. Všimněte si, že nastavení hello změní z tooon místní cloudu.
   
   * Zadejte hodnotu pro **Server**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `ibmserver01:9089`.
   * Zadejte hodnotu pro **databáze**. Můžete například zadat `nwind`.
   * Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
   * Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `informix`.
   * Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
   * Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
7. Vyberte **vytvořit**a potom vyberte **Uložit**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. V hello **InformixgetTables** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
9. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_tables**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat seznam tabulek.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Vytvoření připojení hello
Tento konektor podporuje připojení toodatabase místně a v cloudu hello pomocí hello následující vlastnosti připojení. 

| Vlastnost | Popis |
| --- | --- |
| server |Povinná hodnota. Přijme řetězcovou hodnotu představující adresu TCP/IP nebo alias, ve formátu protokolu IPv4 nebo IPv6, a potom (dvojtečkou oddělený tabulátory) podle čísla portu TCP/IP. |
| Databáze |Povinná hodnota. Přijme řetězcovou hodnotu představující DRDA název pro relační databáze (RDBNAM). Informix přijímá řetězec 128 bajtů (databáze se označuje jako IBM Informix název databáze (dbname)). |
| Ověřování |Volitelné. Přijme hodnotu položky seznamu, Basic nebo Windows (kerberos). |
| uživatelské jméno |Povinná hodnota. Přijme hodnotu řetězce. |
| heslo |Povinná hodnota. Přijme hodnotu řetězce. |
| Brány |Povinná hodnota. Přijme hodnotu položky seznamu, představující hello místní data gateway definované tooLogic aplikací v rámci skupiny úložišť hello. |

## <a name="create-hello-on-premises-gateway-connection"></a>Vytvoření připojení k bráně hello na místě
Tento konektor mohou přistupovat k databázi Informix místně pomocí hello místní data gateway. Další informace naleznete v tématech brány. 

1. V hello **brány** konfigurace podokně, vyberte **políčko** tooenable **připojit prostřednictvím brány**. V tématu hello, změnit nastavení z cloudu tooon místní.
2. Zadejte hodnotu pro **Server**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `ibmserver01:9089`.
3. Zadejte hodnotu pro **databáze**. Můžete například zadat `nwind`.
4. Vyberte hodnotu pro **ověřování**. Vyberte například **základní**.
5. Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `informix`.
6. Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
7. Vyberte hodnotu pro **brány**. Vyberte například **datagateway01**.
8. Vyberte **vytvořit** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Vytvoření připojení cloudu hello
Tento konektor mají přístup k databázi Informix cloudu. 

1. V hello **brány** konfigurace podokně, ponechejte hello **políčko** zakázáno (nepoužité) **připojit prostřednictvím brány**. 
2. Zadejte hodnotu pro **název připojení**. Můžete například zadat `hisdemo2`.
3. Zadejte hodnotu pro **název serveru Informix**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `hisdemo2.cloudapp.net:9089`.
4. Zadejte hodnotu pro **název databáze Informix**. Můžete například zadat `nwind`.
5. Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `informix`.
6. Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
7. Vyberte **vytvořit** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Načíst pomocí vyberte všechny řádky
Toofetch akce aplikace logiky můžete vytvořit všechny řádky v tabulce Informix hello. Tato akce nastaví hello konektor tooprocess příkazem Informix SELECT, jako `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název** (např.) "**InformixgetRows**"), **předplatné**, **skupiny prostředků**, **umístění**, a **plán služby App Service**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - Get řádků (Preview)** .
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**.
7. V hello **připojení** konfigurace podokně, vyberte **vytvořit nový**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. V hello **brány** konfigurace podokně, ponechejte hello **políčko** zakázáno (nepoužité) **připojit prostřednictvím brány**.
   
   * Zadejte hodnotu pro **název připojení**. Můžete například zadat `HISDEMO2`.
   * Zadejte hodnotu pro **název serveru Informix**, hello tvar adresu nebo alias číslo portu dvojtečkou. Můžete například zadat `HISDEMO2.cloudapp.net:9089`.
   * Zadejte hodnotu pro **název databáze Informix**. Můžete například zadat `NWIND`.
   * Zadejte hodnotu pro **uživatelské jméno**. Můžete například zadat `informix`.
   * Zadejte hodnotu pro **heslo**. Můžete například zadat `Password1`.
9. Vyberte **vytvořit** toocontinue.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
11. Volitelně vyberte **zobrazit rozšířené možnosti** toospecify možnosti dotazu.
12. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. V hello **InformixgetRows** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
14. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat seznam řádků.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Přidejte jeden řádek pomocí vložení
Můžete vytvořit logiku aplikace akce tooadd jeden řádek v tabulce Informix. Tato akce dá pokyn hello konektor tooprocess výpis Informix vložit, jako například `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `InformixinsertRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - vložit řádek (Preview)**.
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte tooselect na připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**, typ `Area 99999`a typ `102` pro **REGIONID**. 
10. Vyberte **Uložit**.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. V hello **InformixinsertRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello nový řádek.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Načíst pomocí vyberte jeden řádek
Můžete vytvořit logiku aplikace akce toofetch jeden řádek v tabulce Informix. Tato akce dá pokyn hello konektor tooprocess výpis Informix vyberte kde, jako například `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `InformixgetRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - Get řádků (Preview)** .
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte tooselect stávající připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**. 
10. Volitelně vyberte **zobrazit rozšířené možnosti** toospecify možnosti dotazu.
11. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. V hello **InformixgetRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
13. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat řádek.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Změnit jeden řádek použití aktualizace
Můžete vytvořit logiku aplikace akce toochange jeden řádek v tabulce Informix. Tato akce dá pokyn hello konektor tooprocess výpis Informix aktualizace, jako například `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `InformixupdateRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - aktualizace řádek (Preview)**.
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte tooselect stávající připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**, typ `Updated 99999`a typ `102` pro **REGIONID**. 
10. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. V hello **InformixupdateRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello nový řádek.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Odeberte jeden řádek používání příkazu DELETE
Můžete vytvořit logiku aplikace akce tooremove jeden řádek v tabulce Informix. Tato akce dá pokyn hello konektor tooprocess výpis Informix odstranit, jako například `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky
1. V hello **Tabule start Azure**, vyberte  **+**  (znaménko plus), **Web + mobilní**a potom **aplikace logiky**.
2. Zadejte hello **název**, jako například `InformixdeleteRow`, **předplatné**, **skupiny prostředků**, **umístění**, a **aplikace Služba plán**. Vyberte **Pin toodashboard**a potom vyberte **vytvořit**.

### <a name="add-a-trigger-and-action"></a>Přidání aktivační události a akce
1. V hello **logiku aplikace Návrhář**, vyberte **prázdné LogicApp** v hello **šablony** seznamu.
2. V hello **aktivační události** seznamu, vyberte **opakování**. 
3. V hello **opakování** aktivační událost, vyberte **upravit**, vyberte **frekvence** rozevíracího seznamu tooselect **den**a potom vyberte  **Interval** tootype **7**. 
4. Vyberte hello **+ nový krok** a pak vyberte **přidat akci**.
5. V hello **akce** zadejte **informix** v hello **vyhledejte další akce** textového pole a pak vyberte **Informix - odstranit řádek (Preview)**.
6. V hello **získat řádky (Preview)** akce, vyberte **změnit připojení**. 
7. V hello **připojení** konfigurace podokně, vyberte existující připojení. Vyberte například **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. V hello **název tabulky** seznamu, vyberte hello **šipka dolů**a potom vyberte **oblasti**.
9. Zadejte hodnoty pro všechny požadované sloupce (viz červenou hvězdičkou). Můžete například zadat `99999` pro **AREAID**. 
10. Vyberte **Uložit**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. V hello **InformixdeleteRow** okno, v rámci hello **všechny spustí** v rámci **Souhrn**, vyberte hello uvedené první položka (nejnovější spustit).
12. V hello **spusťte aplikaci logiky** vyberte **podrobnosti o spuštění**. V rámci hello **akce** seznamu, vyberte **Get_rows**. Dokumentaci hello hodnoty **stav**, což by mělo být **úspěšné**. Vyberte hello **vstupy odkaz** tooview hello vstupy. Vyberte hello **výstupy odkaz**a výstupy hello zobrazení, která by měla obsahovat hello odstranit řádek.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Podporované platformy Informix a verze
Tento konektor podporuje následující verze IBM Informix při konfiguraci hello připojení klienta toosupport distribuované relační databáze architektura (DRDA).

* IBM Informix 12.1
* IBM Informix 11.7

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/informix/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).

