---
title: "aaaInstall MongoDB na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall MongoDB ve virtuálním počítači Azure s Windows serverem 2012 R2 vytvořené pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Instalace a konfigurace MongoDB na virtuální počítač s Windows v Azure
[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné. Tento článek vás provede instalací a konfigurací MongoDB na Windows Server 2012 R2 virtuálního počítače (VM) v Azure. Můžete také [nainstalujte MongoDB na virtuální počítač s Linuxem v Azure](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Požadavky
Než nainstalujete a nakonfigurujete MongoDB, můžete potřebovat toocreate virtuálního počítače a, v ideálním případě přidat tooit disku data. Zobrazit hello následující články toocreate virtuální počítač a přidat datový disk:

* Vytvoření virtuálního počítače Windows serveru pomocí [hello portál Azure](quick-create-portal.md) nebo [prostředí Azure PowerShell](quick-create-powershell.md).
* Připojit datový disk tooa virtuálního počítače Windows serveru pomocí [hello portál Azure](attach-managed-disk-portal.md) nebo [prostředí Azure PowerShell](attach-disk-ps.md).

instalace a konfigurace MongoDB, toobegin [přihlásit tooyour virtuálního počítače Windows serveru](connect-logon.md) pomocí vzdálené plochy.

## <a name="install-mongodb"></a>Instalace MongoDB
> [!IMPORTANT]
> Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení. Před nasazením MongoDB tooa produkčním prostředí by měl povolit funkce zabezpečení. Další informace najdete v tématu [MongoDB zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Po připojení tooyour virtuálního počítače pomocí vzdálené plochy, otevřete Internet Explorer z hello **spustit** nabídce hello virtuálních počítačů.
2. Vyberte **doporučuje použít nastavení zabezpečení, ochrany osobních údajů a kompatibility** když Internet Explorer nejprve otevře a klikněte na tlačítko **OK**.
3. Konfigurace rozšířeného zabezpečení aplikace Internet Explorer je standardně povolená. Přidejte hello MongoDB webu toohello seznamu povolených webů:
   
   * Vyberte hello **nástroje** ikonu v pravém horním rohu hello.
   * V **Možnosti Internetu**, vyberte hello **zabezpečení** a pak vyberte hello **Důvěryhodné servery** ikonu.
   * Klikněte na tlačítko hello **lokality** tlačítko. Přidat *https://\*. mongodb.org* toohello seznam důvěryhodných serverů a pak zavřete hello dialogové okno.
     
     ![Konfigurovat nastavení zabezpečení aplikace Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. Procházet toohello [MongoDB - stáhne](http://www.mongodb.org/downloads) stránky (http://www.mongodb.org/downloads).
5. V případě potřeby vyberte hello **komunity serveru** edition a pak vyberte hello nejnovější aktuální stabilní verzi pro Windows Server 2008 R2 64-bit a novější. toodownload hello instalačního programu, klikněte na tlačítko **stahování (msi)**.
   
    ![Stažení instalačního programu MongoDB](./media/install-mongodb/download-mongodb.png)
   
    Spusťte instalační program hello, po dokončení stahování hello.
6. Přečtěte si a přijměte licenční smlouvu hello. Když se zobrazí výzva, vyberte **Complete** nainstalovat.
7. Na úvodní poslední obrazovka, klikněte na tlačítko **nainstalovat**.

## <a name="configure-hello-vm-and-mongodb"></a>Konfigurace hello virtuálních počítačů a MongoDB
1. proměnné cest Hello neaktualizují hello MongoDB instalační službou. Bez hello MongoDB `bin` umístění vaše proměnná path, potřebujete úplná cesta toospecify hello pokaždé, když používáte spustitelný soubor MongoDB. Proměnná tooadd hello umístění tooyour cesty:
   
   * Klikněte pravým tlačítkem na hello **spustit** nabídce a vyberte **systému**.
   * Klikněte na tlačítko **Upřesnit nastavení systému**a potom klikněte na **proměnné prostředí**.
   * V části **systémové proměnné**, vyberte **cesta**a potom klikněte na **upravit**.
     
     ![Nakonfigurujte proměnné cest](./media/install-mongodb/configure-path-variables.png)
     
     Přidat cestu tooyour hello MongoDB `bin` složky. MongoDB je obvykle nainstalovaný v *C:\Program Files\MongoDB*. Ověřte cestu instalace hello na vašem virtuálním počítači. Hello následující příklad přidá hello výchozí MongoDB instalace umístění toohello `PATH` proměnné:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > Být jisti středník před hello tooadd (`;`) tooindicate přidáváte umístění tooyour `PATH` proměnné.

2. Vytváření adresářů MongoDB protokolu a data na datový disk. Z hello **spustit** nabídce vyberte možnost **příkazového řádku**. Následující příklady Hello na jednotce F: Vytvořte hello adresáře
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. MongoDB instance začínat hello následující příkaz, úpravě data tooyour hello cesty a odpovídajícím způsobem protokolu adresáře:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Může trvat několik minut, než MongoDB tooallocate soubory deníku hello a zahájit naslouchání pro připojení. Všechny zprávy protokolu jsou řízené toohello *F:\MongoLogs\mongolog.log* souboru jako `mongod.exe` serveru spustí a přiděluje soubory deníku.
   
   > [!NOTE]
   > příkazový řádek Hello zůstává zaměřují se na tato úloha je spuštěna MongoDB instance. Ponechte hello příkazové okno otevřete toocontinue s MongoDB. Nebo nainstalujte MongoDB jako služba, podle popisu v dalším kroku hello.

4. Robustnější prostředí MongoDB, nainstalujte hello `mongod.exe` jako služba. Vytvoření služby znamená, že nepotřebujete tooleave příkazový řádek s pokaždé, když chcete toouse MongoDB. Vytvoření služby hello následujícím způsobem, upraví hello cesta tooyour protokolu a data adresáře odpovídajícím způsobem:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    Hello předchozí příkaz vytvoří službu s názvem MongoDB, s popisem "Mongo DB". zadáno také Hello následující parametry:
   
   * Hello `--dbpath` možnost určuje umístění hello hello dat adresáře.
   * Hello `--logpath` možnost musí být použité toospecify soubor protokolu, protože spuštěnou službu hello nemá toodisplay okno výstupu příkazu.
   * Hello `--logappend` možnost určuje, že restartování služby hello způsobí, že výstupní tooappend toohello existující soubor protokolu.
   
   toostart hello MongoDB službu, spustit hello následující příkaz:
   
    ```
    net start MongoDB
    ```
   
    Další informace o vytváření hello MongoDB služby najdete v tématu [konfigurace služby systému Windows pro MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-hello-mongodb-instance"></a>Instance MongoDB hello testu
S MongoDB spuštěna jako jedna instance nebo nainstalovat jako službu můžete nyní spustit vytváření a používání vašich databází. hello toostart MongoDB prostředí pro správu, otevřete další okno příkazového řádku z hello **spustit** nabídce a zadejte následující příkaz hello:

```
mongo  
```

Můžete vytvořit seznam hello databáze s hello `db` příkaz. Vložte některá data následujícím způsobem:

```
db.foo.insert( { a : 1 } )
```

Vyhledejte data následujícím způsobem:

```
db.foo.find()
```

Hello výstup je podobné toohello následující ukázka:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Ukončení hello `mongo` konzole následujícím způsobem:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigurace pravidel skupin zabezpečení sítě a brány firewall
Teď, když MongoDB je nainstalovaná a spuštěná, aby se mohli vzdáleně připojit tooMongoDB otevřete port v bráně Windows Firewall. toocreate nový port TCP příchozí pravidlo tooallow 27017, otevřete Správce příkazovém řádku prostředí PowerShell a zadejte hello následující příkaz:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Můžete také vytvořit pravidlo hello pomocí hello **brány Windows Firewall s pokročilým zabezpečením** nástroj pro správu grafického rozhraní. Vytvořte nový port TCP příchozí pravidlo tooallow 27017.

V případě potřeby vytvořte skupinu zabezpečení sítě pravidlo tooallow přístup tooMongoDB z mimo hello existující podsíť virtuální sítě Azure. Můžete vytvořit hello pravidel skupin zabezpečení sítě pomocí hello [portál Azure](nsg-quickstart-portal.md) nebo [prostředí Azure PowerShell](nsg-quickstart-powershell.md). Stejně jako u hello pravidel brány Windows Firewall, povolit TCP port 27017 toohello rozhraní virtuální sítě virtuálního počítače MongoDB.

> [!NOTE]
> TCP port 27017 je výchozí port hello používá MongoDB. Tento port můžete změnit pomocí hello `--port` parametr při spouštění `mongod.exe` ručně nebo ze služby. Pokud změníte hello port, ujistěte se, že tooupdate hello brány Windows Firewall a skupinu zabezpečení sítě pravidla v předchozích krocích hello.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak tooinstall a konfigurace MongoDB na váš virtuální počítač s Windows. Nyní můžete k MongoDB na vašem virtuálním počítači Windows pomocí následující hello advanced témata v hello [dokumentace k MongoDB](https://docs.mongodb.com/manual/).

