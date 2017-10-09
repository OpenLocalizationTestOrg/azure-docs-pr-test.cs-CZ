---
title: "Azure Portal: Vytvoření Azure Database for PostgreSQL | Dokumentace Microsoftu"
description: "Rychlé spuštění Průvodce toocreate a spravovat Azure databáze pro server PostgreSQL pomocí portálu Azure uživatelské rozhraní."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.openlocfilehash: 61753268147d6ea283d1aa5d6baf269d2756d25a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-in-hello-azure-portal"></a>Vytvoření databáze Azure pro PostgreSQL v hello portálu Azure

Azure databázi PostgreSQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze PostgreSQL v cloudu hello. Tento rychlý start ukazuje, jak toocreate Azure databáze pro server PostgreSQL pomocí portálu Azure hello přibližně pět minut.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure
Otevřete webový prohlížeč a přejděte toohello [portálu Microsoft Azure](https://portal.azure.com/). Zadejte vaše přihlašovací údaje toosign toohello portálu. Výchozí zobrazení Hello je váš řídicí panel služby.

## <a name="create-an-azure-database-for-postgresql"></a>Vytvoření Azure Database for PostgreSQL

Server Azure Database for PostgreSQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Hello server je vytvořen v rámci [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Postupujte podle těchto kroků toocreate databázi Azure pro PostgreSQL server:
1.  Klikněte na tlačítko hello **nový** tlačítko (+) najít na hello levém horním rohu hello portálu Azure.
2.  Vyberte **databáze** z hello **nový** a vyberte **databáze Azure pro PostgreSQL** z hello **databáze** stránky.
 ![Databáze Azure pro PostgreSQL - vytvořit databázi hello](./media/quickstart-create-database-portal/1-create-database.png)

3.  Vyplňte hello nového serveru podrobnosti formuláře s hello následující informace, jak je znázorněno na hello předcházející bitové kopie:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Název serveru |*mypgserver-20170401*|Zvolte jedinečný název serveru, který identifikuje váš server Azure Database for PostgreSQL. název domény Hello *postgres.database.azure.com* je název serveru připojením toohello přinášejí tooconnect aplikace k. název serveru Hello může obsahovat jenom malá písmena, číslice a znak hello pomlčku (-) a musí obsahovat od 3 do 63 znaků.
    Předplatné|*Vaše předplatné*|Hello předplatné Azure, který má toouse pro váš server. Pokud máte více předplatných, zvolte hello příslušné předplatné, ve kterém se fakturuje hello prostředků pro.
    Skupina prostředků|*myresourcegroup*| Můžete vytvořit nový název skupiny prostředků nebo použít některý ze stávajících ve vašem předplatném.
    Přihlašovací jméno správce serveru |*mylogin*| Zkontrolujte vlastní toouse účtu přihlášení, při připojování serveru toohello. Hello správce přihlašovací jméno nemůže být 'azure_superuser', 'azure_pg_admin', "admin", 'správce, "kořenový", 'hosta' nebo 'veřejné' a nesmí začínat na 'pg_'.
    Heslo |*Nějaké si zvolte* | Vytvořte nové heslo pro účet správce serveru hello. Musí obsahovat z 8 too128 znaků. Heslo musí obsahovat znaky ze tří z následujících kategorií hello: velká písmena, malá písmena anglické abecedy, čísla (0-9) a jiné než alfanumerické znaky (!, $, #, %, atd.).
    Umístění|*Hello oblast nejbližší tooyour uživatelů*| Vyberte hello umístění, které je nejblíže tooyour uživatele.
    Verze PostgreSQL|*Vyberte nejnovější verzi hello*| Vyberte nejnovější verzi hello, pokud nemáte konkrétní požadavky.
    Cenová úroveň | **Basic**, **50 výpočetních jednotek**, **50 GB** | Klikněte na tlačítko **cenová úroveň** toospecify hello služby vrstvy a úroveň výkonu pro novou databázi. Zvolte úroveň Basic hello karty v horní části hello. Klikněte na tlačítko hello levého konce hello výpočetní jednotky posuvníku tooadjust hello hodnota toohello minimem k dispozici pro tento rychlý start. Klikněte na tlačítko **Ok** toosave hello cenová úroveň výběru. V tématu hello následující snímek obrazovky.
    | Toodashboard PIN kód | Zaškrtnout | Zkontrolujte hello **Pin toodashboard** možnost tooallow snadné sledování serveru na stránce hello front řídicí panel portálu Azure.

  > [!IMPORTANT]
  > Hello přihlašovací jméno správce serveru a heslo, které tady zadáte jsou požadované toolog toohello serveru a její databáze dále v této úvodní. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

    ![Azure databázi PostgreSQL - vybrat hello cenové úrovně](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Klikněte na tlačítko **vytvořit** tooprovision hello serveru. Zřizování trvá několik minut, až minut too20 maximální.

5.  Na panelu nástrojů hello, klikněte na tlačítko **oznámení** procesu nasazení toomonitor hello.
 ![Azure Database for PostgreSQL – zobrazení oznámení](./media/quickstart-create-database-portal/3-notifications.png)
   
  Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databáze je výchozí databáze určené výhradně pro uživatele, nástroje a aplikace třetích stran. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Hello databáze Azure pro službu PostgreSQL vytvoří brána firewall na úrovni serveru hello. Tato brána firewall brání externí aplikace a nástroje pro připojení serveru toohello a všechny databáze na serveru hello, pokud pravidlo brány firewall není vytvořená tooopen hello brány firewall pro konkrétní IP adresy. 

1.  Po dokončení nasazení hello umístit si server. V případě potřeby ho můžete vyhledat. Klikněte například na **všechny prostředky** z nabídky na levé straně hello a zadejte název serveru hello (jako je například hello *mypgserver 20170401*) toosearch pro nově vytvořený server. Klikněte na název serveru uvedené v výsledek hledání hello. Hello **přehled** stránky pro váš server otevře a poskytuje možnosti pro další konfiguraci.
 
    ![Azure Database for PostgreSQL – hledání názvu serveru](./media/quickstart-create-database-portal/4-locate.png)

2.  Na stránce server hello vyberte **zabezpečení připojení**. 
    ![Azure Database for PostgreSQL – vytvoření pravidla brány firewall](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  V části hello **pravidla brány Firewall** záhlaví, klikněte na tlačítko hello prázdné textového pole v hello **název pravidla** sloupec toobegin vytvoření pravidla brány firewall hello. 

    Pro tento rychlý start můžeme povolit všechny IP adresy do serveru hello vyplněním hello textového pole v každém sloupci s hello následující hodnoty:

    Název pravidla | Počáteční IP adresa | Koncová IP adresa 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na hello horním panelu nástrojů stránky zabezpečení hello připojení, klikněte na tlačítko **Uložit**. Počkejte pár chvil a Všimněte si hello oznámení zobrazující, že aktualizace zabezpečení připojení byla dokončena úspěšně než budete pokračovat.

    > [!NOTE]
    > Připojení tooyour Azure databáze pro PostgreSQL server komunikovat přes port 5432. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 5432 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nebudete moct tooconnect tooyour serveru, pokud vaše IT oddělení otevře port 5432.
    >

## <a name="get-hello-connection-information"></a>Získat informace o připojení hello

Při vytvoření našeho serveru Azure Database for PostgreSQL se vytvoří i výchozí databáze **postgres**. tooconnect tooyour databázový server, musíte tooremember hello celého serveru název a správce přihlašovací údaje. Může mít poznamenat tyto hodnoty dříve v hello rychlý start článku. V případě, že jste to udělali není, budete moci snadno najít hello název a přihlašovací informace o serveru ze stránky přehled server hello v hello portálu Azure.

1. Otevřete stránku **Přehled** vašeho serveru. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
    Ukazatele myši každé pole a toohello vpravo od textu hello se zobrazí ikona kopírování hello. Klikněte na ikonu kopírování hello jako potřebné toocopy hello hodnoty.

 ![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-toopostgresql-database-using-psql-in-cloud-shell"></a>Připojit databáze tooPostgreSQL pomocí psql v prostředí cloudu

Existuje řada aplikací můžete použít tooconnect tooyour Azure databáze pro PostgreSQL server. Nejprve použijeme hello psql nástroj příkazového řádku tooillustrate jak tooconnect toohello serveru.  Můžete použít webový prohlížeč a hello prostředí cloudu Azure podle postupu popsaného tady bez hello potřebovat tooinstall žádný další software. Pokud máte nástroj psql hello nainstalovány místně na vlastní počítače, můžete zde také připojit.

1. Spusťte hello prostředí cloudu Azure prostřednictvím terminálu ikonu hello v horním navigačním podokně hello.

   ![Azure Database for PostgreSQL – ikona terminálu Azure Cloud Shell](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Otevře se v prohlížeči, které příkazy prostředí bash tootype Hello prostředí cloudu Azure.

   ![Azure Database for PostgreSQL – příkazový řádek Bash služby Azure Shell](./media/quickstart-create-database-portal/8-bash.png)

3. Na příkazovém řádku prostředí cloudu hello připojte databáze tooa ve vaší databázi Azure pro PostgreSQL server zadáním hello psql příkazového řádku hello zelená řádku.

    Hello následující formát je použité tooconnect tooan Azure databáze pro server PostgreSQL s hello [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Například následující příkaz hello připojuje server tooan příklad:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    --host | *název serveru* | Zadejte hello hodnota názvu serveru, která byla použita při předtím vytvořili hello databáze Azure pro PostgreSQL. Náš ukázkový server v příkladu je mypgserver-20170401.postgres.database.azure.com. Použití hello plně kvalifikovaný název domény (\*. postgres.database.azure.com) jako v příkladu hello. Pokud si nepamatujete název serveru, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení. 
    --port | **5432** | Vždy používejte port 5432 při připojování tooAzure databáze pro PostgreSQL. 
    --username | *přihlašovací jméno správce serveru* |Zadejte hello serveru správce přihlašovací uživatelské jméno, když máte vytvořený hello databáze Azure pro PostgreSQL. Pokud si nepamatujete hello uživatelské jméno, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení.  Formát Hello je  *username@servername* .
    --dbname | **postgres** | Název databáze pomocí hello výchozí vygenerované systémem *postgres* pro první připojení hello. Později vytvoříte vlastní databázi.

    Po spuštěný příkaz psql hello s vlastními hodnotami parametrů jste heslo správce serveru výzvami tootype hello. Toto heslo je hello stejné, že jste zadali při vytváření hello server. 

    Parametr psql |Navrhovaná hodnota|Popis
    ---|---|---
    heslo | *vaše heslo správce* | Všimněte si, hello typové heslo, které znaky nejsou zobrazeny na hello bash výzva. Stiskněte klávesu enter po zadali všechny znaky tooauthenticate hello a připojení.

    Po připojení, zobrazí nástroj psql hello postgres řádku, kde zadáte příkazy sql. Ve výstupu hello počáteční připojení může se zobrazit upozornění, protože hello psql v hello prostředí cloudu Azure může být jinou verzi než hello databáze Azure pro verzi serveru PostgreSQL. 
    
    Příklad výstupu nástroje psql:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Pokud brána firewall hello není nakonfigurovaný tooallow hello IP adresu hello prostředí cloudu Azure, hello došlo k následující chybě:
    > 
    > "psql: FATAL: no pg_hba.conf entry for host "138.91.195.82", user "mylogin", database "postgres", SSL on FATAL:  SSL connection is required. (psql: ZÁVAŽNÁ CHYBA: nenalezen žádný záznam pg_hba.conf pro hostitele 138.91.195.82, uživatele mylogin, databázi postgres, ZÁVAŽNÁ CHYBA SSL: vyžaduje se připojení SSL.) Zadejte možnosti SSL a zkuste to znovu.
    > 
    > Chyba hello tooresolve, ujistěte se, hello serveru konfigurace odpovídá hello kroky hello *nakonfigurujte pravidlo brány firewall na úrovni serveru* hello článku.

4.  Vytvořte prázdnou databázi na hello výzva zadáním hello následující příkaz:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    příkaz Hello může trvat pár chvil toocomplete. 

5.  Na příkazovém řádku hello spustit následující příkaz tooswitch připojení toohello nově vytvořený databáze hello **mypgsqldb**.
    ```bash
    \c mypgsqldb
    ```

6.  Zadejte \q a stiskněte klávesu ENTER tooquit psql. Po dokončení můžete zavřít hello prostředí cloudu Azure.

Teď už máte připojením toohello Azure databáze PostgreSQL a vytvořit prázdnou uživatelské databáze. Pokračujte toohello další části tooconnect pomocí jiné běžné nástroje pgAdmin.

## <a name="connect-toopostgresql-database-using-pgadmin"></a>Připojit databáze tooPostgreSQL pomocí pgAdmin

tooconnect tooAzure PostgreSQL serveru pomocí hello grafického uživatelského rozhraní nástroje _pgAdmin_
1.  Spusťte hello _pgAdmin_ aplikace na klientském počítači. _pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.
2.  Klikněte na tlačítko hello **přidejte nový Server** na ikonu hello **rychlé odkazy** oddílu v centru hello hello stránka řídicího panelu.
3.  V hello **vytvoření - serveru** dialogové okno **Obecné** zadejte jedinečný popisný název pro hello server, jako například **Azure PostgreSQL Server**.
![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  V hello **vytvoření - serveru** dialogové okno, **připojení** kartě, použijte nastavení hello uvedeného a klikněte na tlačítko **Uložit**.
   ![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    Parametr pgAdmin |Navrhovaná hodnota|Popis
    ---|---|---
    Název nebo adresa hostitele | *název serveru* | Zadejte hello hodnota názvu serveru, která byla použita při předtím vytvořili hello databáze Azure pro PostgreSQL. Náš ukázkový server v příkladu je mypgserver-20170401.postgres.database.azure.com. Použití hello plně kvalifikovaný název domény (\*. postgres.database.azure.com) jako v příkladu hello. Pokud si nepamatujete název serveru, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení. 
    Port | **5432** | Vždy používejte port 5432 při připojování tooAzure databáze pro PostgreSQL.  
    Databáze údržby | **postgres** | Název databáze pomocí hello výchozí vygenerované systémem *postgres*.
    Uživatelské jméno | *přihlašovací jméno správce serveru* | Zadejte hello serveru správce přihlašovací uživatelské jméno, když máte vytvořený hello databáze Azure pro PostgreSQL. Pokud si nepamatujete hello uživatelské jméno, postupujte podle kroků hello v hello předchozí část tooget hello informace o připojení. Formát Hello je  *username@servername* .
    Heslo | *vaše heslo správce* |  Hello heslo jste zvolili, když jste vytvořili hello server dříve v tento rychlý start.
    Role | *ponechte prázdné* | Ne musejí tooprovide roli název v tomto okamžiku. Hello pole zůstat prázdné.
    Režim SSL | Vyžadovat | Ve výchozím nastavení jsou všechny servery Azure PostgreSQL vytvořeny se zapnutým vynucováním SSL. tooturn vypnout vynucování SSL, najdete v podrobnostech v [vynucování SSL](./concepts-ssl-connection-security.md).
    
5.  Klikněte na **Uložit**.
6.  V levém podokně Prohlížeč hello rozbalte hello **servery** uzlu. Vyberte server, například **Azure PostgreSQL Server** a klikněte na tlačítko tooconnect tooit.
7. Rozbalte uzel serveru hello a pak rozbalte **databáze** v něm. Hello seznamu by měla obsahovat vaše stávající *postgres* databáze a všechny nově vytvořeného uživatele databáze, jako například *mypgsqldb*, kterou jsme vytvořili v předchozí části hello. Nezapomeňte, že na jeden server se službou Azure Database for PostgreSQL můžete vytvořit více databází.
8. Klikněte pravým tlačítkem na **databáze**, zvolte hello **vytvořit** nabídky a klikněte na tlačítko **databáze**.
9.  Zadejte název databáze zvoleného v hello **databáze** pole, jako třeba *mypgsqldb* v příkladu hello. 
10. Vyberte hello **vlastníka** hello databáze z rozevíracího seznamu hello. Zvolte přihlašovací jméno správce serveru, jako je *mylogin* v našem příkladu.
10. Klikněte na tlačítko **Uložit** toocreate novou prázdnou databázi.
11. V hello **prohlížeče** podokně, najdete v části hello databáze, které jste vytvořili v hello seznam databází v části název serveru.
 ![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků
Vyčištění hello prostředky, které jste vytvořili v rychlý start hello buď odstraněním hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md), který zahrnuje všechny hello prostředky ve skupině prostředků hello nebo odstraněním prostředků jeden server hello, pokud chcete, aby tookeep hello Další prostředky beze změn.

> [!TIP]
> Další rychlé starty v této kolekci vycházejí z tohoto rychlého startu. Pokud máte v plánu toocontinue na toowork s následné rychlých průvodců, proveďte není vyčistit hello prostředky vytvořené v tento rychlý start. Pokud neplánujete toocontinue, použijte následující kroky toodelete prostředky vytvořené tento rychlý start v hello portál Azure hello.

toodelete hello celé skupiny prostředků včetně hello nově vytvořený serveru:
1.  Vyhledejte vaší skupiny prostředků v hello portálu Azure. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název vaší skupiny prostředků, jako je například v našem příkladu hello **myresourcegroup**.
2.  Na stránce vaší skupiny prostředků klikněte na **Odstranit**. Pak typ hello název vaší skupiny prostředků, jako je například v našem příkladu **myresourcegroup**, v hello odstranění tooconfirm textové pole a pak klikněte na tlačítko **odstranit**.

Nebo místo toho toodelete hello nově vytvořený serveru:
1.  Najděte váš server v hello portál Azure, pokud jste ho otevřete. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili.
2.  Na hello **přehled** klikněte na tlačítko hello **odstranit** tlačítko na horním podokně hello.
![Azure Database for PostgreSQL – odstranění serveru](./media/quickstart-create-database-portal/12-delete.png)
3.  Zkontrolujte název serveru hello chcete toodelete a zobrazit hello databází v něm, které se vztahuje. Zadejte název serveru hello textového pole, jako je například v našem příkladu **mypgserver 20170401**a potom klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
