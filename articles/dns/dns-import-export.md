---
title: "aaaImport a export zónu domény souboru tooAzure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak tooimport a export DNS zóny souboru tooAzure DNS pomocí Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a>Import a export souboru zóny DNS pomocí hello Azure CLI 1.0 

Tento článek vás provede jak hello tooimport a export souborů zóny DNS pro Azure DNS pomocí Azure CLI 1.0.

## <a name="introduction-toodns-zone-migration"></a>Úvod tooDNS zóny migrace

Soubor zóny DNS je textový soubor, který obsahuje podrobnosti o každé systému DNS (Domain Name) záznamu v zóně hello. Postupuje standardní formát, takže je vhodné pro přenos mezi systémy DNS záznamy DNS. Pomocí souboru zóny je rychlé, spolehlivé a pohodlný způsob tootransfer zóny DNS do nebo z Azure DNS.

Azure DNS podporuje import a export souborů zóny pomocí hello rozhraní příkazového řádku Azure (CLI). Import souboru zóny je **není** prostřednictvím prostředí Azure PowerShell nebo hello portál Azure momentálně nepodporuje.

Hello Azure CLI 1.0 je nástroj příkazového řádku a platformy pro správu služby Azure. Je k dispozici pro platformy Windows, Mac a Linux hello z hello [stránky Azure stahování](https://azure.microsoft.com/downloads/). Podpora více platforem je důležité pro import a export souborů zóny, protože hello nejběžnější název serverového softwaru [vazby](https://www.isc.org/downloads/bind/), obvykle běží na systému Linux.

> [!NOTE]
> Aktuálně neexistují dvě verze hello rozhraní příkazového řádku Azure. CLI1.0 je založena na Node.js a má příkazy počínaje "azure".
> CLI2.0 je založena na Python a má příkazy počínaje "az". Při importu souboru zóny je podporovaná v obou verzích, doporučujeme používat příkazy CLI1.0 hello, jak je popsáno v této stránce.

## <a name="obtain-your-existing-dns-zone-file"></a>Získat existující soubor zóny DNS

Před importem souboru zóny DNS do Azure DNS, musíte tooobtain kopii souboru zóny hello. Hello zdroj tohoto souboru závisí na je aktuálně hostitelem zóny DNS hello.

* Pokud zónu DNS je hostitelem služby partnera (například doménového registrátora, vyhrazené poskytovatele hostingu DNS nebo poskytovatele alternativní cloudu), měl by poskytnout služby hello možnost toodownload souboru zóny DNS hello.
* Pokud DNS systému Windows je hostitelem zóny DNS, je hello výchozí složku pro soubory zóny hello **%systemroot%\system32\dns**. soubor zóny tooeach úplná cesta Hello také ukazuje na hello **Obecné** karty hello DNS konzoly.
* Pokud je hostovaná zóny DNS pomocí vazby, hello umístění souboru hello zóny pro každou zónu jsou uvedeny v souboru konfigurace vazby hello **named.conf**.

> [!NOTE]
> Zóny soubory stahované z GoDaddy mají mírně nestandardní formát. Tuto funkci potřebujete toocorrect před importem tyto soubory zóny do Azure DNS.
>
> Názvy DNS v hello RDATA každý záznam DNS jsou určené jako plně kvalifikované názvy, ale nemají ukončující "." To znamená, že se interpretují jinými systémy DNS jako relativních názvů. Je třeba tooedit hello zóny souboru tooappend hello ukončení "." tootheir názvy před importem do Azure DNS.
>
> Například hello záznam CNAME "v doméně contoso.com CNAME www 3600" by mělo být změněno příliš "contoso.com IN CNAME www 3600."
> (s ukončující ".").

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Import souboru zóny DNS do Azure DNS

Import souboru zóny vytvoří nové zóny v Azure DNS, pokud ještě neexistuje. Pokud zóna hello již existuje, hello sad záznamů v souboru zóny hello je potřeba sloučit s hello existující sady záznamů.

### <a name="merge-behavior"></a>Sloučení chování

* Ve výchozím nastavení jsou sloučeny stávající a nové sady záznamů. Jsou identické záznamy v sadě záznamů sloučené zrušte duplicitní.
* Můžete také zadáním hello `--force` možnost, hello nahradí proces importu stávajícího záznamu, nastaví se nové sady záznamů. Existující sady záznamů, které nemají odpovídající záznam v souboru importované zóny hello se neodeberou.
* Když jsou sloučeny sady záznamů, hello čas toolive (TTL) dříve existující sady záznamů se používá. Když `--force` se používá, se používá hello TTL hello nové sady záznamů.
* Začátek parametry záznam Authority (SOA) (s výjimkou `host`) jsou vždy převzat ze souboru hello importované zóny, bez ohledu na to, jestli se `--force` se používá. Podobně pro hello název serveru sady záznamů na vrcholu zóny hello, hello TTL je vždy převzat ze souboru importované zóny hello.
* Importovaný záznam CNAME nenahrazuje existující CNAME záznam s hello stejný název, pokud hello `--force` je zadán parametr.
* Když dojde ke konfliktu mezi záznam CNAME a jiný záznam hello stejný název, ale jiné zadejte (bez ohledu na to, který je existující nebo nové), se uchovávají hello stávajícího záznamu. Je nezávislé na použití hello `--force`.

### <a name="additional-information-about-importing"></a>Další informace o importu

Hello následující poznámky k poskytují další technické podrobnosti o hello zóny importu.

* Hello `$TTL` – direktiva je volitelný a je podporované. Pokud ne `$TTL` – direktiva je zadána, jsou importovány záznamy bez explicitního TTL nastavit výchozí tooa TTL 3 600 sekund. Když dva zaznamenává v hello stejné sady záznamů zadejte jiný TTLs, hello nižší hodnota se používá.
* Hello `$ORIGIN` – direktiva je volitelný a je podporované. Pokud ne `$ORIGIN` nastavena výchozí hello používá, je název zóny hello jako zadaného na příkazovém řádku hello (plus ukončení hello ".").
* Hello `$INCLUDE` a `$GENERATE` direktivy nejsou podporovány.
* Jsou podporovány těchto typů záznamů: A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.
* Hello záznam SOA je vytvářena automaticky nástrojem Azure DNS, když je vytvořena zóna. Když importujete soubor zóny, jsou všechny parametry SOA převzat ze souboru zóny hello *s výjimkou* hello `host` parametr. Tento parametr používá hello hodnotu poskytovanou infrastrukturou Azure DNS. Je to proto, že tento parametr musí odkazovat toohello název primárního serveru poskytuje Azure DNS.
* záznam názvového serveru Hello nastavit na vrcholu zóny hello se také automaticky vytvoří Azure DNS při vytvoření zóny hello. Pouze hello TTL této sady záznamů je naimportována. Tyto záznamy obsahovat názvy názvových serverů hello poskytuje Azure DNS. data záznamů Hello není přepsány hello hodnoty obsažené v souboru importované zóny hello.
* Během verzi Public Preview Azure DNS podporuje pouze jeden řetězec záznamů TXT. Nahrazován záznamů TXT jsou být zřetězených a zkrácený too255 znaků.

### <a name="cli-format-and-values"></a>Formát rozhraní příkazového řádku a hodnoty

Formát Hello hello rozhraní příkazového řádku Azure příkaz tooimport zónu DNS je:

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

Hodnoty:

* `<resource group>`je název skupiny prostředků hello hello hello zóny v Azure DNS.
* `<zone name>`je název hello hello zóny.
* `<zone file name>`je hello cesta a název toobe soubor zóny hello importovat.

Pokud zóna s tímto názvem neexistuje ve skupině prostředků hello, vytvoří se pro vás. Pokud hello zóně už existuje, hello sady importovaných záznamů jsou sloučeny s existující sady záznamů. toooverwrite hello existující sady záznamů, použijte hello `--force` možnost.

tooverify hello formát souboru zóny bez jeho použití hello ve skutečnosti import `--parse-only` možnost.

### <a name="step-1-import-a-zone-file"></a>Krok 1. Importovat soubor zóny

tooimport souboru zóny pro zónu hello **contoso.com**.

1. Přihlaste se tooyour předplatného Azure pomocí Azure CLI 1.0 hello.

    ```azurecli
    azure login
    ```

2. Vyberte předplatné hello místo toocreate nové zóny DNS.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS je jen správce prostředků služby Azure, takže hello rozhraní příkazového řádku Azure musí být vypnuté tooResource režimu správce.

    ```azurecli
    azure config mode arm
    ```

4. Než použijete hello služba Azure DNS, je nutné zaregistrovat poskytovatel prostředků Microsoft.Network vaše předplatné toouse hello. (Jde o jednorázovou operaci u každého předplatného.)

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. Pokud nemáte již, musíte taky toocreate skupinu prostředků Resource Manager.

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. zóny hello tooimport **contoso.com** ze souboru hello **contoso.com.txt** do nové zóny DNS ve skupině prostředků hello **myresourcegroup**, spusťte příkaz hello `azure network dns zone import`.<BR>Tento příkaz načte soubor zóny hello a analyzovat ho. příkaz Hello provede řadu příkazů hello Azure DNS služby toocreate hello zóny a nastaví všechny hello záznamu v zóně hello. příkaz Hello sestavy průběhu v okně konzoly hello společně s žádné chyby nebo upozornění. Protože sady záznamů jsou vytvářeny v řadě, může trvat několik minut tooimport souboru velké zóny.

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a>Krok 2. Ověřte hello zóny

tooverify hello zóny DNS po importu souboru hello, můžete použít kterékoli z hello následující metody:

* Pomocí následujícího příkazu příkazového řádku Azure CLI hello můžete vytvořit seznam záznamů hello:

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* Můžete vytvořit seznam záznamů hello pomocí rutiny prostředí PowerShell hello `Get-AzureRmDnsRecordSet`.
* Můžete použít `nslookup` tooverify překlad názvů pro záznamy hello. Protože ještě není přidělena hello zóny, toospecify hello správné názvových serverů Azure DNS je třeba explicitně. Hello následující příklad ukazuje, jak přiřazovat názvy názvových serverů tooretrieve hello toohello zóny. IT také ukazuje, jak tooquery hello "www" zaznamenání pomocí `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Krok 3. Aktualizovat delegování DNS

Po ověření, že zóna hello importu správně, musíte tooupdate hello DNS delegování toopoint toohello názvové servery Azure DNS. Další informace najdete v článku hello [aktualizovat delegování DNS hello](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportovat souboru zóny DNS z Azure DNS.

Formát Hello hello rozhraní příkazového řádku Azure příkaz tooimport zónu DNS je:

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

Hodnoty:

* `<resource group>`je název skupiny prostředků hello hello hello zóny v Azure DNS.
* `<zone name>`je název hello hello zóny.
* `<zone file name>`je hello cesta a název toobe soubor zóny hello exportovali.

Jako s importem hello zóny, musíte nejdřív toosign v, vyberte předplatné a nakonfigurujte režimu Resource Manager toouse hello rozhraní příkazového řádku Azure.

### <a name="tooexport-a-zone-file"></a>tooexport soubor zóny

1. Přihlaste se tooyour předplatného Azure pomocí hello rozhraní příkazového řádku Azure.

    ```azurecli
    azure login
    ```

2. Vyberte předplatné hello místo toocreate zóny DNS.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS je služba Azure Resource Manager. Hello rozhraní příkazového řádku Azure musí být vypnuté tooResource režimu správce.

    ```azurecli
    azure config mode arm
    ```

4. tooexport hello existující zóny DNS **contoso.com** ve skupině prostředků **myresourcegroup** toohello soubor **contoso.com.txt** (ve složce hello aktuální), spusťte `azure network dns zone export`. Tento příkaz, že volání hello tooenumerate služby Azure DNS sady záznamů v zóně hello a exportovat soubor zóny hello výsledky tooa vazby kompatibilní.

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
