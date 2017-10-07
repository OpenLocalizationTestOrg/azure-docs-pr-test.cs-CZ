---
title: "Rychlý start: Vytvoření serveru Azure Database for MySQL – Azure Portal | Dokumentace Microsoftu"
description: "Tento článek postup vás provede pomocí portálu Azure tooquickly hello vytvořit ukázkové databáze Azure pro server databáze MySQL přibližně pět minut."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.openlocfilehash: d5754fe7a6f0f0f4b3fa19d456c4e15e64ca396c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Vytvoření serveru Azure Database for MySQL pomocí portálu Azure Portal
Azure databáze pro databázi MySQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze MySQL v cloudu hello. Tento rychlý start ukazuje, jak toocreate Azure databáze pro server MySQL pomocí portálu Azure hello přibližně pět minut. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure
Otevřete webový prohlížeč a přejděte toohello [portálu Microsoft Azure](https://portal.azure.com/). Zadejte vaše přihlašovací údaje toosign toohello portálu. Výchozí zobrazení Hello je váš řídicí panel služby.

## <a name="create-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Hello server je vytvořen v rámci [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Postupujte podle těchto kroků toocreate databázi Azure pro MySQL server:

1. Klikněte na tlačítko hello **nový** tlačítko (+) najít na hello levém horním rohu hello portálu Azure.

2. Vyberte **databáze** z hello **nový** a vyberte **Azure Database pro databázi MySQL** z hello **databáze** stránky. Můžete také zadat **MySQL** v hello nové stránky vyhledávání pole toofind hello služby.
![Azure Portal – nový – databáze - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Vyplňte hello nového serveru podrobnosti formuláře s hello následující informace, jak je znázorněno na hello předcházející bitové kopie:

    **Nastavení** | **Navrhovaná hodnota** | **Popis pole** 
    ---|---|---
    Název serveru | myserver4demo | Zvolte jedinečný název serveru, který identifikuje váš server Azure Database for MySQL. název domény Hello *mysql.database.azure.com* je název serveru připojením toohello přinášejí tooconnect aplikace k. název serveru Hello může obsahovat jenom malá písmena, číslice a znak hello pomlčku (-) a musí obsahovat od 3 do 63 znaků.
    Předplatné | Vaše předplatné | Hello předplatné Azure, který má toouse pro váš server. Pokud máte více předplatných, zvolte hello příslušné předplatné, ve kterém se fakturuje hello prostředků pro.
    Skupina prostředků | myresourcegroup | Můžete vytvořit nový název skupiny prostředků nebo použít některý ze stávajících ve vašem předplatném.
    Přihlašovací jméno správce serveru | myadmin | Zkontrolujte vlastní toouse účtu přihlášení, při připojování serveru toohello. přihlašovací jméno správce Hello nelze 'azure_superuser', "admin", 'správce, 'root', 'hosta' nebo 'veřejné'.
    Heslo | *Nějaké si zvolte* | Vytvořte nové heslo pro účet správce serveru hello. Musí obsahovat z 8 too128 znaků. Heslo musí obsahovat znaky ze tří z následujících kategorií hello: velká písmena, malá písmena anglické abecedy, čísla (0-9) a jiné než alfanumerické znaky (!, $, #, %, atd.).
    Potvrzení hesla | *Nějaké si zvolte*| Potvrďte heslo pro účet správce hello.
    Umístění | *Hello oblast nejbližší tooyour uživatelů*| Vyberte hello umístění, které je nejblíže uživatele tooyour nebo jiné aplikace na platformě Azure.
    Verze | *Vyberte nejnovější verzi hello*| Vyberte nejnovější verzi hello, pokud nemáte konkrétní požadavky.
    Cenová úroveň | **Basic**, **50 výpočetních jednotek**, **50 GB** | Klikněte na tlačítko **cenová úroveň** toospecify hello služby vrstvy a úroveň výkonu pro novou databázi. Zvolte **úroveň Basic** v hello karty v horní části hello. Klikněte na tlačítko hello levého konce hello **výpočetní jednotky** posuvníku tooadjust hello hodnota toohello alespoň částka k dispozici pro tento rychlý start. Klikněte na tlačítko **Ok** toosave hello cenová úroveň výběru. V tématu hello následující snímek obrazovky.
    Toodashboard PIN kód | Zaškrtnout | Zkontrolujte hello **Pin toodashboard** možnost tooallow snadné sledování serveru na stránce hello front řídicí panel portálu Azure.

    > [!IMPORTANT]
    > Hello přihlašovací jméno správce serveru a heslo, které zde určíte jsou požadované toolog toohello serveru a její databáze později v tento rychlý start. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.
    > 

    ![Portál Azure – vytvoření databáze MySQL tím, že poskytuje hello vyžaduje vstup formuláře](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Klikněte na tlačítko **vytvořit** tooprovision hello serveru. Zřizování trvá několik minut, až minut too20 maximální.
   
5.  Na panelu nástrojů hello, klikněte na tlačítko **oznámení** procesu nasazení hello toomonitor (ikonu zvonku).

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Hello databáze Azure pro službu MySQL vytvoří brána firewall na úrovni serveru hello. Tato brána firewall brání externí aplikace a nástroje pro připojení serveru toohello a všechny databáze na serveru hello, pokud pravidlo brány firewall není vytvořená tooopen hello brány firewall pro konkrétní IP adresy. 

1.  Po dokončení nasazení hello umístit si server. V případě potřeby ho můžete vyhledat. Klikněte například na **všechny prostředky** z nabídky na levé straně hello a zadejte název serveru hello (jako je například hello *myserver4demo*) toosearch pro nově vytvořený server. Klikněte na název serveru uvedené v výsledek hledání hello. Hello **přehled** stránky pro váš server otevře a poskytuje možnosti pro další konfiguraci.

2. Na stránce server hello vyberte **zabezpečení připojení**.

3.  V části hello **pravidla brány Firewall** záhlaví, klikněte na tlačítko hello prázdné textového pole v hello **název pravidla** sloupec toobegin vytvoření pravidla brány firewall hello. 

    Pro tento rychlý start můžeme povolit všechny IP adresy do serveru hello vyplněním hello textového pole v každém sloupci s hello následující hodnoty:

    Název pravidla | Počáteční IP adresa | Koncová IP adresa 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na horním panelu nástrojů hello Dobrý den **zabezpečení připojení** klikněte na tlačítko **Uložit**. Počkejte pár chvil a Všimněte si hello oznámení zobrazující, že aktualizace zabezpečení připojení byla dokončena úspěšně než budete pokračovat.

    > [!NOTE]
    > TooAzure připojení databáze pro databázi MySQL komunikovat přes port 3306. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 3306 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nebudete moct tooconnect tooyour serveru, pokud vaše IT oddělení otevře port 3306.
    > 

## <a name="get-hello-connection-information"></a>Získat informace o připojení hello
tooconnect tooyour databázový server, musíte tooremember hello celého serveru název a správce přihlašovací údaje. Může mít uvedené výše v článku rychlý start hello tyto hodnoty. V případě, že jste to udělali není, budete moci snadno najít hello server název a přihlašovací informace ze serveru hello **přehled** stránky nebo hello **vlastnosti** stránku hello portálu Azure.

1. Otevřete stránku **Přehled** vašeho serveru. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**. 
    Ukazatele myši každé pole a toohello vpravo od textu hello se zobrazí ikona kopírování hello. Klikněte na ikonu kopírování hello jako potřebné toocopy hello hodnoty.

    V tomto příkladu je název serveru hello *myserver4demo.mysql.database.azure.com*, a je přihlašovací jméno správce serveru hello * myadmin@myserver4demo *.

## <a name="connect-toomysql-using-mysql-command-line-tool"></a>Připojit tooMySQL pomocí nástroje příkazového řádku mysql
Existuje řada aplikací můžete použít tooconnect tooyour Azure databáze pro server databáze MySQL. Použijeme nejprve hello [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) příkazového řádku nástroje tooillustrate jak tooconnect toohello serveru.  Můžete použít webový prohlížeč a hello prostředí cloudu Azure podle postupu popsaného tady bez hello potřebovat tooinstall žádný další software. Pokud máte nástroj mysql hello nainstalovány místně na vlastní počítače, můžete zde také připojit.

1. Spusťte hello prostředí cloudu Azure prostřednictvím terminálu ikonu hello (> _) na hello top napravo od hello Azure portálu webové stránky.

2. Otevře se v prohlížeči, které příkazy prostředí bash tootype Hello prostředí cloudu Azure.

    ![Příkazový řádek – příklad příkazového řádku mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. Na příkazovém řádku prostředí cloudu hello připojte tooyour Azure databáze MySQL serveru zadáním hello mysql příkazového řádku hello zelená řádku.

    Hello formátu je použité tooconnect tooan Azure databáze pro server databáze MySQL s nástroj mysql hello:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Například následující příkaz hello připojuje server tooour příklad:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    Parametr mysql |Navrhovaná hodnota|Popis
    ---|---|---
    --host | *název serveru* | Zadejte hello hodnota názvu serveru, která byla použita při dříve vytvořili hello Azure Database pro databázi MySQL. Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použití hello plně kvalifikovaný název domény (\*. mysql.database.azure.com) jako v příkladu hello. Pokud si nepamatujete název serveru, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení. 
    --user | *přihlašovací jméno správce serveru* |Zadejte hello serveru správce přihlašovací uživatelské jméno, když máte vytvořený hello Azure Database pro databázi MySQL. Pokud si nepamatujete hello uživatelské jméno, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení.  Formát Hello je * username@servername *.
    --password | *počkejte na zobrazení výzvy* | Zobrazí se výzva příliš "Zadejte heslo", po které zadejte příkaz hello. Po zobrazení výzvy zadejte hello stejné heslo, které jste zadali při vytváření hello server.  Poznámka: hello zadali heslo, které znaky nejsou zobrazeny na hello bash výzvy při jejich zadávání. Stiskněte klávesu enter po zadali všechny znaky tooauthenticate hello a připojení.

   Po připojení, zobrazí nástroj mysql hello `mysql>` výzva pro vás tootype příkazy. 

    Příklad výstupu nástroje mysql:
    ```bash
    Welcome toohello MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Pokud brána firewall hello není nakonfigurovaný tooallow hello IP adresu hello prostředí cloudu Azure, hello došlo k následující chybě:
    >
    > Chyba 2003 (28000): Klient s IP adresou 123.456.789.0 tooaccess hello serveru není povoleno.
    >
    > Chyba hello tooresolve, ujistěte se, hello serveru konfigurace odpovídá hello kroky hello *nakonfigurujte pravidlo brány firewall na úrovni serveru* hello článku.

4. Zobrazení serveru stav tooensure hello připojení je funkční. Typ `status` v hello mysql > výzvu po připojení.
    ```sql
    status
    ```

   > [!TIP]
   > Další příkazy najdete v [Referenční příručce k MySQL 5.7 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Vytvořit prázdnou databázi v hello mysql > výzva zadáním hello následující příkaz:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    příkaz Hello může trvat pár chvil toocomplete. 

    V rámci serveru Azure Database for MySQL můžete vytvořit jednu nebo několik databází. Můžete vyjádřit výslovný toocreate jednu databázi na serveru tooutilize všechny prostředky hello nebo vytvořit více databází tooshare hello prostředky. Neexistuje žádné omezení toohello počet databází, které lze vytvořit, ale více databází sdílet hello stejné prostředky serveru. 

6. Seznam hello databáze na hello mysql > výzva zadáním hello následující příkaz:

    ```sql
    SHOW DATABASES;
    ```

7.  Typ `\q` a potom stiskněte klávesu ENTER tooquit hello mysql nástroj. Po dokončení můžete zavřít hello prostředí cloudu Azure.

Teď už máte připojení toohello Azure Database pro databázi MySQL a vytvořit prázdnou uživatelské databáze. Pokračovat v další části toorepeat toohello podobné toohello tooconnect cvičení stejný server pomocí jiného běžné nástroje MySQL Workbench.

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Připojení serveru toohello pomocí nástroje hello MySQL Workbench grafického uživatelského rozhraní
server databáze MySQL tooAzure tooconnect pomocí hello grafického uživatelského rozhraní nástroje MySQL Workbench:

1.  Spusťte hello MySQL Workbench aplikace na klientském počítači. MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).

2.  V **nastavit připojení k nové** dialogovém okně zadejte následující informace hello **parametry** karty:

    ![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
    |---|---|---|
    |   Název připojení | Ukázkové připojení | Zadejte popisek pro toto připojení. |
    | Způsob připojení | Standard (TCP/IP) | Standard (TCP/IP) je dostačující. |
    | Název hostitele | *název serveru* | Zadejte hello hodnota názvu serveru, která byla použita při dříve vytvořili hello Azure Database pro databázi MySQL. Náš ukázkový server v příkladu je myserver4demo.mysql.database.azure.com. Použití hello plně kvalifikovaný název domény (\*. mysql.database.azure.com) jako v příkladu hello. Pokud si nepamatujete název serveru, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení.  |
    | Port | 3306 | Vždy používejte port 3306 při připojování tooAzure databáze pro databázi MySQL. |
    | Uživatelské jméno |  *přihlašovací jméno správce serveru* | Zadejte hello serveru správce přihlašovací uživatelské jméno, když máte vytvořený hello Azure Database pro databázi MySQL. Uživatelské jméno v našem příkladu je myadmin@myserver4demo. Pokud si nepamatujete hello uživatelské jméno, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení. Formát Hello je * username@servername *.
    | Heslo | vaše heslo | Klikněte na tlačítko Uložit v trezoru... tlačítko toosave hello heslo. |

    Klikněte na tlačítko **Test připojení** tootest, pokud jsou správně nakonfigurovány všechny parametry. Klikněte na tlačítko OK toosave hello připojení. 

    > [!NOTE]
    > SSL je vyžadována ve výchozím nastavení na vašem serveru a vyžaduje další konfigurace v pořadí tooconnect úspěšně. V tématu [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](./howto-configure-ssl.md).  Pokud chcete toodisable SSL pro tento rychlý start, navštivte hello portál Azure a klikněte na tlačítko hello připojení zabezpečení stránky toodisable hello vynutit SSL připojení přepínací tlačítko.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Vyčištění hello prostředky, které jste vytvořili v rychlý start hello buď odstraněním hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md), který zahrnuje všechny hello prostředky ve skupině prostředků hello nebo odstraněním prostředků jeden server hello, pokud chcete, aby tookeep hello Další prostředky beze změn.

> [!TIP]
> Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud máte v plánu toocontinue na toowork s následné rychlých průvodců, proveďte není vyčistit hello prostředky vytvořené v tento rychlý start. Pokud neplánujete toocontinue, použijte následující kroky toodelete všechny prostředky vytvořeny ve tento rychlý start v hello portál Azure hello.
>

toodelete hello celé skupiny prostředků včetně hello nově vytvořený serveru:
1.  Vyhledejte vaší skupiny prostředků v hello portálu Azure. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název vaší skupiny prostředků, jako je například v našem příkladu hello **myresourcegroup**.
2.  Na stránce vaší skupiny prostředků klikněte na **Odstranit**. Pak typ hello název vaší skupiny prostředků, jako je například v našem příkladu **myresourcegroup**, v hello odstranění tooconfirm textové pole a pak klikněte na tlačítko **odstranit**.

Nebo místo toho toodelete hello nově vytvořený serveru:
1.  Najděte váš server v hello portál Azure, pokud jste ho otevřete. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili.
2.  Na hello **přehled** klikněte na tlačítko hello **odstranit** tlačítko na horním podokně hello.
![Azure Database for MySQL – odstranění serveru](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Zkontrolujte název serveru hello chcete toodelete a zobrazit hello databází v něm, které se vztahuje. Zadejte název serveru hello textového pole, jako je například v našem příkladu **myserver4demo**a potom klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Návrh první databáze Azure Database for MySQL](./tutorial-design-database-using-portal.md)

