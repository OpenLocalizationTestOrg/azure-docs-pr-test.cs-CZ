---
title: "Výpočetní aaaAzure - rozšíření diagnostiky Linux | Microsoft Docs"
description: "Jak tooconfigure hello Azure Linux diagnostiky rozšíření (LAD) toocollect metrik a protokolování událostí, z virtuálních počítačů Linux běží v Azure."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a>Pomocí rozšíření diagnostiky Linux toomonitor metriky a protokoly

Tento dokument popisuje verze 3.0 a novějších systému hello rozšíření diagnostiky Linux.

> [!IMPORTANT]
> Informace o 2.3 a starší verze najdete v tématu [tento dokument](./classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Úvod

Hello rozšíření diagnostiky Linux pomáhá monitorování hello stav uživatele z virtuálního počítače s Linuxem systémem Microsoft Azure. Má hello následující možnosti:

* Shromažďuje metriky výkonu systému z hello virtuálních počítačů a ukládá je do určité tabulky v účtu úložiště určený.
* Načte události protokolu z syslog a ukládá je do určité tabulky v hello určený účet úložiště.
* Umožňuje uživatelům toocustomize hello data metriky, které jsou shromažďovány a nahrát.
* Umožňuje uživatelům toocustomize hello syslog zařízení a úrovně závažnosti události, které jsou shromažďovány a nahrát.
* Umožňuje uživatelům tooupload protokol příslušné soubory tooa určené úložiště tabulky.
* Podporuje odesílání metriky a protokolu událostí tooarbitrary EventHub koncových bodů a objekty BLOB ve formátu JSON v hello určený účet úložiště.

Toto rozšíření pracuje s obou modelech nasazení Azure.

## <a name="installing-hello-extension-in-your-vm"></a>Instaluje se rozšíření hello virtuálního počítače

Toto rozšíření můžete povolit pomocí rutin prostředí Azure PowerShell text hello, skripty rozhraní příkazového řádku Azure nebo Azure nasazení šablony. Další informace najdete v tématu [rozšíření funkce](./extensions-features.md).

Hello portál Azure nelze použít tooenable nebo nakonfigurovat LAD 3.0. Místo toho nainstaluje a nakonfiguruje verze 2.3. Výstrahy a portálu Azure grafy pracovat s daty v obou verzích hello rozšíření.

Tyto pokyny k instalaci a [ke stažení ukázkové konfiguraci](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) konfigurace LAD 3.0 na:

* zachycení a úložiště hello metriky stejné jako byly poskytované LAD 2.3;
* zaznamenat užitečné sadu metriky systému souborů, nové tooLAD 3.0;
* zaznamenat hello výchozí syslog kolekci povoleno podle LAD 2.3;
* Povolte hello Azure portálu prostředí pro vytváření grafů a výstrahy na metriky virtuálního počítače.

zaváděná konfigurace Hello je jenom jako příklad; Upravit toosuit vašich potřeb.

### <a name="prerequisites"></a>Požadavky

* **Azure Linux Agent verze 2.2.0 nebo novější**. Většina virtuálních počítačů Linux Azure Galerie Image zahrnují verze 2.2.7 nebo novější. Spustit `/usr/sbin/waagent -version` tooconfirm hello verze nainstalované v hello virtuálních počítačů. Pokud hello virtuální počítač běží starší verze agenta hosta hello, postupujte podle [tyto pokyny](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate ho.
* **Azure CLI**. [Nastavit hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) prostředí na váš počítač.
* Hello wget příkazu, pokud ji nemáte: Spusťte `sudo apt-get install wget`.
* Stávající předplatné Azure a existující úložiště účtu v něm toostore hello data.

### <a name="sample-installation"></a>Ukázka instalace

Zadejte správné parametry hello na hello první tři řádky, a poté spusťte tento skript jako kořenového adresáře:

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

Adresa URL Hello hello Ukázková konfigurace a její obsah, jsou toochange subjektu. Stáhněte si kopii souboru JSON hello nastavení portálu a přizpůsobit pro své potřeby. Všechny šablony nebo automatizace, které vytvoříte měli používat vlastní kopie místo stahování pokaždé, když tuto adresu URL.

### <a name="updating-hello-extension-settings"></a>Aktualizuje se nastavení rozšíření hello

Po změně chráněné nebo veřejné nastavení, je nasadit toohello virtuálních počítačů tak, že spustíte hello stejný příkaz. Pokud nic změnit v nastavení hello, odešlou hello aktualizovat nastavení toohello rozšíření. LAD hello konfigurace znovu načte a restartuje sám sebe.

### <a name="migration-from-previous-versions-of-hello-extension"></a>Migrace z předchozích verzí rozšíření hello

nejnovější verze rozšíření hello Hello je **3.0**. **Všechny starší verze (2.x) jsou zastaralé a může být publikování na nebo po 31. července 2018**.

> [!IMPORTANT]
> Toto rozšíření zavádí narušující změny toohello konfigurace rozšíření hello. Jedna taková změna byla provedená tooimprove hello zabezpečení hello rozšíření; v důsledku toho zpětné kompatibility s 2.x nebylo možné udržovat. Navíc se liší od vydavatele hello verze 2.x hello hello vydavatel rozšíření pro tuto příponu.
>
> toomigrate z 2.x toothis novou verzi hello rozšíření, je nutné odinstalovat původní rozšíření hello (pod původním názvem vydavatele hello) a potom nainstalovat verzi 3 hello rozšíření.

Doporučení:

* Nainstalujte rozšíření hello automatické podverze upgradu povolena.
  * V modelu nasazení classic virtuální počítače zadejte '3.*' jako verze hello při instalaci rozšíření hello prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu.
  * Při nasazení Azure Resource Manager modelu virtuálních počítačů, zahrnují ' "autoUpgradeMinorVersion": true, v hello šablony nasazení virtuálního počítače.
* Použijte nový nebo jiný účet úložiště pro LAD 3.0. Existuje několik malých nekompatibility mezi LAD 2.3 a LAD 3.0, které usnadňují sdílení problémových účet:
  * LAD 3.0 ukládá události procesu syslog v tabulce s jiným názvem.
  * Hello counterSpecifier řetězce pro `builtin` metriky rozdílnou LAD 3.0.

## <a name="protected-settings"></a>Chráněné nastavení

Tato sada informace o konfiguraci obsahuje citlivé informace, které by měly být chráněné z veřejného zobrazení, například přihlašovací údaje úložiště. Tato nastavení jsou přenášená tooand uložené hello rozšíření v šifrovaném tvaru.

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Name (Název) | Hodnota
---- | -----
storageAccountName | Hello název účtu úložiště hello, ve kterém je rozšířením hello zapisovat data.
storageAccountEndPoint | koncový bod (volitelné) hello identifikace hello cloudu, ve které hello existuje účet úložiště. Chybí-li toto nastavení, LAD výchozí toohello veřejného cloudu Azure, `https://core.windows.net`. Tuto hodnotu nastavit toouse účet úložiště v Azure v Německu, Azure Government nebo Azure China odpovídajícím způsobem.
storageAccountSasToken | [Tokenu SAS účtu](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) pro služby objektů Blob a tabulky (`ss='bt'`), použít toocontainers a objekty (`srt='co'`), která uděluje přidat, vytvářet, seznam, aktualizovat a oprávnění k zápisu (`sp='acluw'`). Proveďte *není* zahrnují hello úvodní otazník (?).
mdsdHttpProxy | (volitelné) HTTP proxy informace potřebné tooenable hello rozšíření tooconnect toohello zadaný účet úložiště a koncového bodu.
sinksConfig | (volitelné) Podrobnosti o alternativní cíle toowhich metriky a události mohou být zajišťovány. Hello konkrétní podrobnosti o každém jímku dat nepodporuje hello rozšíření jsou popsané v následujících hello částech.

Můžete snadno vytvořit token SAS hello požadované prostřednictvím hello portálu Azure.

1. Vyberte toowhich účet úložiště pro obecné účely hello chcete toowrite rozšíření hello
1. Vyberte "Sdílený přístupový podpis" z hello nastavení součástí levé nabídce hello
1. Ujistěte se, jak už bylo popsáno příslušné části hello
1. Klikněte na tlačítko "Generovat SAS" hello.

![Bitové kopie](./media/diagnostic-extension/make_sas.png)

Kopírování hello vygeneruje SAS do pole storageAccountSasToken hello; odebrat hello úvodní otazník ("?").

### <a name="sinksconfig"></a>sinksConfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Tento volitelný oddíl definuje další cíle, které toowhich hello rozšíření odesílá hello informace, které shromáždí. pole "podřízený" Hello obsahuje objekt pro každý další data jímky. Hello atributu "typ" Určuje hello další atributy v objektu hello.

Element | Hodnota
------- | -----
jméno | Řetězec se používá podřízený toothis toorefer jinde v konfiguraci rozšíření hello.
type | Hello typ jímky definovaný. Určuje instance tohoto typu hello ostatní hodnoty (pokud existuje).

Verze 3.0 hello rozšíření diagnostiky Linux podporuje dva typy podřízený: EventHub a JsonBlob.

#### <a name="hello-eventhub-sink"></a>podřízený EventHub Hello

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

zadání "sasURL" Hello obsahuje hello úplnou adresu URL, včetně tokenu SAS pro hello centra událostí toowhich data by měla být zveřejněny. LAD vyžaduje SAS pojmenování zásadu, která umožňuje odesílání hello deklarace identity. Příklad:

* Vytvořit obor názvů Event Hubs s názvem`contosohub`
* Vytvoření centra událostí v oboru názvů hello názvem`syslogmsgs`
* Vytvoření zásady sdíleného přístupu na hello centra událostí s názvem `writer` , umožňuje hello odesílat deklarace identity

Pokud jste vytvořili SAS funkční až půlnoci času UTC na 1. ledna 2018 hello sasURL hodnota může být:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Další informace o generování tokeny SAS pro Event Hubs naleznete v tématu [této webové stránce](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="hello-jsonblob-sink"></a>podřízený JsonBlob Hello

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

Data směrované podřízený JsonBlob tooa se ukládají do objektů BLOB v úložišti Azure. Každá instance LAD vytvoří každou hodinu pro každý název podřízený objekt blob. Každý objekt blob vždy obsahuje syntakticky pole JSON objektu. Nové položky se přidají atomicky toohello pole. Objekty BLOB jsou uloženy v kontejneru s hello stejný název jako jímku hello. Hello pravidla úložiště Azure pro názvy objektů blob kontejneru použít názvy toohello JsonBlob jímky: mezi 3 a 63 malé alfanumerické znaky ASCII nebo pomlčky.

## <a name="public-settings"></a>Nastavení veřejné

Tato struktura obsahuje různé bloky nastavení, které řídí hello shromažďované nástrojem hello rozšíření. Každé nastavení je volitelné. Pokud zadáte `ladCfg`, musíte zadat také `StorageAccount`.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

Element | Hodnota
------- | -----
StorageAccount | Hello název účtu úložiště hello, ve kterém je rozšířením hello zapisovat data. Musí být stejný název, jako jsou zadány v hello hello [chráněné nastavení](#protected-settings).
mdsdHttpProxy | (volitelné) Stejné jako hello [chráněné nastavení](#protected-settings). hodnota veřejného Hello je přepsat privátní hodnotou hello, pokud nastavení. Umístěte nastavení proxy serveru, které obsahují tajného klíče, jako jsou hesla, v hello [chráněné nastavení](#protected-settings).

Hello zbývající prvky jsou podrobně popsané v v následující části hello.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

Tento volitelný struktura ovládací prvky hello shromažďování metrik a protokoly pro doručení toohello dat služby a tooother metrik Azure jímky. Je nutné zadat buď `performanceCounters` nebo `syslogEvents` nebo obojí. Je nutné zadat hello `metrics` struktura.

Element | Hodnota
------- | -----
eventVolume | (volitelné) Ovládací prvky hello počet oddílů v rámci tabulku úložiště hello vytvořili. Musí mít jednu z `"Large"`, `"Medium"`, nebo `"Small"`. Pokud není zadaný, hello výchozí hodnota je `"Medium"`.
sampleRateInSeconds | (volitelné) hello výchozí interval mezi kolekce nezpracovaná (neagregovaným) metrik. Hello nejmenší podporované vzorkovací frekvence je 15 sekund. Pokud není zadaný, hello výchozí hodnota je `15`.

#### <a name="metrics"></a>Průzkumníku metrik

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Element | Hodnota
------- | -----
resourceId | ID prostředku Azure Resource Manager Hello hello virtuálního počítače nebo škálování virtuálních počítačů hello nastavit hello toowhich, ke které patří virtuální počítač. Toto nastavení musí být zadaná také pokud žádné podřízený JsonBlob se používá v konfiguraci hello.
scheduledTransferPeriod | frekvence Hello, kdy jsou agregovaná metrika toobe počítaný a přenést tooAzure metriky, vyjádřené jako interval času je 8601. doba přenosu nejmenší Hello je 60 sekund, který je PT1M. Je nutné zadat alespoň jeden scheduledTransferPeriod.

Ukázky hello metriky uvedený v oddílu hello čítače výkonu jsou shromažďována každých 15 sekund nebo na hello vzorkovací frekvence explicitně definován čítače hello. Pokud se zobrazí více scheduledTransferPeriod frekvence (jako třeba hello), každé agregace se počítá samostatně.

#### <a name="performancecounters"></a>čítače výkonu

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Tento volitelný oddíl ovládací prvky kolekce hello metrik. Nezpracovaná ukázky jsou agregována pro každou [scheduledTransferPeriod](#metrics) tooproduce tyto hodnoty:

* střední
* minimální
* Maximální počet
* shromážděné poslední hodnota
* počet nezpracovaných vzorků, které se používá toocompute hello agregace

Element | Hodnota
------- | -----
jímky | (volitelné) Čárkami oddělený seznam názvy jímky toowhich LAD odesílá agregované výsledky metriky. Všechny agregovaných metrik jsou publikované tooeach uvedené jímky. V tématu [sinksConfig](#sinksconfig). Příklad: `"EHsink1, myjsonsink"`.
type | Identifikuje hello skutečné zprostředkovatele hello metriky.
– Třída | Společně s "čítač" identifikuje hello určité metriky v rámci obor názvů zprostředkovatele hello.
Čítač | Společně s "class" identifikuje hello určité metriky v rámci obor názvů zprostředkovatele hello.
counterSpecifier | Identifikuje hello určité metriky v rámci oboru názvů hello metrik Azure.
Podmínka | (volitelné) Použije konkrétní instanci hello objekt toowhich hello metrika vybere nebo vybere hello agregace napříč všemi instancemi tohoto objektu. Další informace najdete v tématu hello [ `builtin` definice metrik](#metrics-supported-by-builtin).
sampleRate | JE 8601 interval, který nastaví četnost Dobrý den, kdy se shromažďují nezpracovaná ukázky pro tuto metriku. Pokud není nastavená, interval sběru hello se nastavuje hodnotu hello [sampleRateInSeconds](#ladcfg). Hello nejkratší podporované vzorkovací frekvence je 15 sekund (PT15S).
jednotka | By měl být jeden z těchto řetězců: "Count", "Bajtů", "Seconds", "Procenta", "CountPerSecond", "BytesPerSecond", "Milisekundu". Definuje hello jednotky pro metriku hello. Spotřebitelé dat shromážděných hello očekávat, že hello shromažďují data hodnoty toomatch tuto jednotku. LAD ignoruje toto pole.
displayName | Popisek (v jazyce hello určeného přidružené nastavení národního prostředí v hello) toobe Hello připojit toothis data v Azure metriky. LAD ignoruje toto pole.

Hello counterSpecifier je libovolný identifikátor. Příjemci metrik, jako jsou hello grafů, portálu Azure a výstrah funkce, použijte counterSpecifier jako hello "klíč", který identifikuje metriky nebo instanci metriky. Pro `builtin` metriky, doporučujeme použít counterSpecifier hodnoty, které začínají `/builtin/`. Pokud shromažďujete konkrétní instanci metriky, doporučujeme, abyste že připojení hello identifikátor hello instance toohello counterSpecifier hodnoty. Příklady:

* `/builtin/Processor/PercentIdleTime`-Doba nečinnosti, po průměrem všech jader
* `/builtin/Disk/FreeSpace(/mnt)`-Volného místa pro systém souborů /mnt hello
* `/builtin/Disk/FreeSpace`-Volného místa průměrem všech připojené systémy

LAD ani hello portál Azure očekává hello counterSpecifier hodnotu toomatch žádnému vzoru. Být konzistentní v tom, jak vytvořit counterSpecifier hodnoty.

Pokud zadáte `performanceCounters`, LAD vždy zapíše data tooa tabulky v úložišti Azure. Vám může mít hello stejná data zapsaná tooJSON objekty BLOB a/nebo Event Hubs, ale nelze zakázat ukládání dat tooa tabulky. Všechny instance hello nakonfigurované rozšíření diagnostiky toouse hello stejný účet úložiště, název a koncový bod přidat jejich metriky a protokoly toohello stejné tabulky. Pokud jsou příliš mnoho virtuálních počítačů zápis toohello, který může omezit stejného oddílu tabulky, Azure zapíše toothat oddílu. nastavení příčiny položky toobe eventVolume Hello rozloženy 1 (malá), 10 (střední) nebo 100 různých oddílů (velká). "Střední" je obvykle dostatečná není omezen tooensure provoz. Funkce Azure metriky Hello hello portál Azure používá hello data v této tabulce tooproduce grafy nebo tootrigger výstrahy. Název tabulky Hello je hello zřetězení tyto řetězce:

* `WADMetrics`
* Hello "scheduledTransferPeriod" pro hello agregovat hodnoty, které jsou uložené v tabulce hello
* `P10DV2S`
* Datum, v podobě hello "RRRRMMDD", který změní každých 10 dní

Mezi příklady patří `WADMetricsPT1HP10DV2S20170410` a `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEvents

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Tento volitelný oddíl ovládací prvky kolekce hello protokolu událostí z syslog. Pokud je vynechán části hello, nejsou vůbec zachytit události procesu syslog.

kolekce syslogEventConfiguration Hello má jeden záznam pro každý mechanismus syslog, které vás zajímají. Pokud minSeverity je "Žádný" pro konkrétní zařízení, nebo pokud tohoto zařízení nezobrazí v elementu hello vůbec, jsou zachyceny žádné události z tohoto zařízení.

Element | Hodnota
------- | -----
jímky | Čárkami oddělený seznam názvy jímky toowhich jednotlivých protokolů, které se události publikují. Všechny události protokolu odpovídající omezení hello v syslogEventConfiguration jsou publikované tooeach uvedené jímky. Příklad: "EHforsyslog"
facilityName | Název zařízení syslog (například "protokolu\_uživatele" nebo "protokolu\_LOCAL0"). Najdete v části "zařízení" hello hello [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam hello.
minSeverity | Úroveň závažnosti syslog (například "protokolu\_chyba" nebo "protokolu\_informace"). Najdete v části "úroveň" hello hello [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam hello. rozšíření Hello zaznamená zařízení toohello událostí odeslaných na nebo výše hello zadané úrovně.

Pokud zadáte `syslogEvents`, LAD vždy zapíše data tooa tabulky v úložišti Azure. Vám může mít hello stejná data zapsaná tooJSON objekty BLOB a/nebo Event Hubs, ale nelze zakázat ukládání dat tooa tabulky. Hello dělení chování pro tuto tabulku je hello stejné popsané pro `performanceCounters`. Název tabulky Hello je hello zřetězení tyto řetězce:

* `LinuxSyslog`
* Datum, v podobě hello "RRRRMMDD", který změní každých 10 dní

Mezi příklady patří `LinuxSyslog20170410` a `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Spuštění libovolné ovládací prvky této volitelné části [OMI](https://github.com/Microsoft/omi) dotazy.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Element | Hodnota
------- | -----
obor názvů | obor názvů OMI hello (volitelné) v rámci které hello se má provést dotaz. Pokud tento parametr nezadáte, hello výchozí hodnota je "kořenový/scx", implementované hello [System Center a platformy zprostředkovatelé](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
query | Hello OMI toobe dotaz spustit.
Tabulka | (volitelné) hello tabulky úložiště Azure, v hello určený účet úložiště (viz [chráněné nastavení](#protected-settings)).
frequency | (volitelné) hello počet sekund mezi spuštění dotazu hello. Výchozí hodnota je 300 (5 minut); minimální hodnota je 15 sekund.
jímky | (volitelné) Čárkami oddělený seznam názvů další jímky toowhich nezpracovaná ukázka metriky výsledků je nutné ji publikovat. Žádné agregace ukázek nezpracovaná se počítá pomocí přípony hello nebo metrik Azure.

Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.

### <a name="filelogs"></a>fileLogs

Ovládací prvky hello zaznamenání souborů protokolu. LAD zaznamená nové řádky textu, jako jsou zapsané toohello soubor a zapíše tootable řádků nebo všechny zadané jímky (JsonBlob nebo EventHub).

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Element | Hodnota
------- | -----
Soubor | Úplná cesta toobe souboru protokolu hello Hello sledovaná a zachycení. Název musí být Hello pathname jeden soubor; nemůže název adresáře nebo obsahovat zástupné znaky.
Tabulka | tabulky (volitelné) hello úložiště Azure, v úložišti hello určené účtu (jako je zadaný v konfiguraci hello chráněné), do které nové řádky z hello "tail" hello souboru se zapisují.
jímky | (volitelné) Čárkami oddělený seznam názvů další jímky toowhich protokolu řádky odeslána.

Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.

## <a name="metrics-supported-by-hello-builtin-provider"></a>Metriky podporované poskytovatelem builtin hello

Zprostředkovatel metriky builtin Hello je zdroj metriky nejvíce zajímavé tooa širokou škálu uživatele. Tyto metriky spadají do pěti široký třídy:

* Procesor
* Memory (Paměť)
* Síť
* Systém souborů
* Disk

### <a name="builtin-metrics-for-hello-processor-class"></a>předdefinované metriky pro hello procesoru – třída

Hello procesoru třída metrik poskytuje informace o využití procesoru v hello virtuálních počítačů. Při agregování procenta, výsledkem hello je hello průměr mezi všechny procesory. V dva základní virtuální počítač Pokud jednoho jádra bylo 100 % zaneprázdněn a hello dalších 100 % nečinnosti, hello uvádět, že PercentIdleTime by bylo 50. Pokud byl 50 % zaneprázdněn pro každý základní hello stejné období, hello hlášené výsledek by také bylo 50. V čtyři základní virtuální počítač, pomocí jednoho jádra 100 % zaneprázdněn a hello ostatní nečinnosti, hello oznámilo že percentidletime by 75.

Čítač | Význam
------- | -------
PercentIdleTime | Procento času období hello agregace, procesory byly provádění hello jádra nečinné smyčky
percentProcessorTime | Procento doby provádění jiných než nečinných vláken
PercentIOWaitTime | Procento doby čekání na vstupně-výstupní operace toocomplete
PercentInterruptTime | Procento doby provádění hardware a software přerušení a DPC (odložených volání procedur)
PercentUserTime | Jiných než nečinných času období agregace hello hello procento času stráveného v uživatele více se střední důležitostí
PercentNiceTime | Jiných než nečinných času stráví hello procento snížený (dobrý) důležitostí
PercentPrivilegedTime | Jiných než nečinných času stráví hello procento v privilegovaném režimu režimu (jádra)

Hello první čtyři čítače by měl součet too100 %. Hello poslední tři čítače také součet too100 %; jejich rozdělit hello součet PercentProcessorTime, PercentIOWaitTime a PercentInterruptTime.

nastavit tooobtain jednu metriku agregovat do všech procesorů `"condition": "IsAggregate=TRUE"`. tooobtain metriky pro specifické procesory, například hello druhý logický procesor čtyři základní virtuální počítač, nastavte `"condition": "Name=\\"1\\""`. Logický procesor čísla jsou v rozsahu hello `[0..n-1]`.

### <a name="builtin-metrics-for-hello-memory-class"></a>předdefinované metriky pro hello paměti – třída

Hello paměti třída metrik poskytuje informace o využití paměti, stránkování a vzájemná záměna.

Čítač | Význam
------- | -------
AvailableMemory | Dostupná fyzická paměť v MiB
PercentAvailableMemory | Dostupná fyzická paměť v procentech celkové paměti
UsedMemory | Použití fyzické paměti (MiB)
PercentUsedMemory | Fyzické paměti používané jako procento celkové paměti
PagesPerSec | Celkový počet stránkování (čtení a zápis)
PagesReadPerSec | Čtení stránek ze záložního úložiště (odkládacího souboru, soubor programu, mapovaný soubor, atd.)
PagesWrittenPerSec | Uložení stránek zapsaných toobacking (odkládací soubor, mapovaný soubor, atd.)
AvailableSwap | Nepoužívané odkládacího prostoru (MiB)
PercentAvailableSwap | Nepoužívané odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru
UsedSwap | Využití odkládacího souboru (MiB)
PercentUsedSwap | Používané místo odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru

Tato třída metrik obsahuje pouze jednu instanci. atribut "podmínky" Hello neexistuje užitečné nastavení a by měl být vynechán.

### <a name="builtin-metrics-for-hello-network-class"></a>předdefinované metriky pro hello sítě – třída

Hello sítě třída metrik poskytuje informace o síťové aktivity na rozhraní jednotlivým síťovým od spuštění počítače. LAD nevystavuje metriky šířky pásma, které je možné načíst z hostitele metriky.

Čítač | Význam
------- | -------
BytesTransmitted | Celkový počet bajtů odeslaných od spuštění
BytesReceived | Celkový počet bajtů přijatých od spuštění
BytesTotal | Celkový počet bajtů odesílané nebo přijímané od spuštění počítače
PacketsTransmitted | Celkový počet paketů odeslaných od spuštění
PacketsReceived | Celkový počet paketů přijatých od spuštění
TotalRxErrors | Počet chyb, obdrží od spuštění počítače
TotalTxErrors | Počet chyb přenosu od spuštění počítače
TotalCollisions | Počet kolizí hlášené hello síťové porty od spuštění počítače

 I když tato třída je instance, nepodporuje LAD zaznamenávání metriky sítě s agregovat přes všechna síťová zařízení. nastavení metriky hello tooobtain pro určité rozhraní, jako je například eth0, `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-hello-filesystem-class"></a>předdefinované metriky pro hello Filesystem – třída

Hello třída Filesystem metrik poskytuje informace o použití systému souborů. Absolutní a procentuální hodnoty jsou hlášeny, jako by byly zobrazené tooan obyčejnou uživatel (není uživatel root).

Čítač | Význam
------- | -------
FreeSpace | Dostupné místo na disku v bajtech
UsedSpace | Využité místo na disku v bajtech
PercentFreeSpace | Procento volného místa
PercentUsedSpace | Procento využitého prostoru
PercentFreeInodes | Procentuální hodnota nepoužívané uzlů Inode
PercentUsedInodes | Procentuální hodnota přidělené (používán) uzlů Inode sčítají mezi všechny systémy
BytesReadPerSecond | Načíst počet bajtů za sekundu
BytesWrittenPerSecond | Bajtů zapsaných za sekundu
BytesPerSecond | Bajtů číst nebo zapisovat za sekundu
ReadsPerSecond | Operace čtení za sekundu
WritesPerSecond | Zápis operací za sekundu
TransfersPerSecond | Operace čtení nebo zápisu za sekundu

Agregované hodnoty mezi všechny systémy souborů je možné získat nastavení `"condition": "IsAggregate=True"`. Hodnoty pro konkrétní připojeného souboru systém, například "/ mnt", je možné získat nastavení `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-hello-disk-class"></a>předdefinované metriky pro hello disku – třída

Hello disku třída metrik poskytuje informace o využití disku zařízení. Ve statistikách použít toohello celé jednotky. Pokud existují více systémy souborů na zařízení, hello čítače pro toto zařízení jsou, efektivně, agregovat přes všechny z nich.

Čítač | Význam
------- | -------
ReadsPerSecond | Operace čtení za sekundu
WritesPerSecond | Zápis operací za sekundu
TransfersPerSecond | Celkový počet operací za sekundu
AverageReadTime | Průměrný počet sekund na operace čtení
AverageWriteTime | Průměrný počet sekund na operace zápisu
AverageTransferTime | Průměrný počet sekund na operaci
AverageDiskQueueLength | Průměrný počet operací zařazených do fronty disku
ReadBytesPerSecond | Počet bajtů přečtených za sekundu
WriteBytesPerSecond | Počet bajtů zapsaných za sekundu
BytesPerSecond | Počet bajtů přečtených nebo zapsaných za sekundu

Agregované hodnoty všech disků je možné získat nastavení `"condition": "IsAggregate=True"`. nastavit tooget informace pro konkrétní zařízení (například/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Instalace a konfigurace LAD 3.0 prostřednictvím rozhraní příkazového řádku

Za předpokladu, že jsou chráněný nastavení v souboru hello PrivateConfig.json a veřejné konfigurační informace v PublicConfig.json, spusťte tento příkaz:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

příkaz Hello předpokládá, že používáte hello Správa prostředků Azure režim (arm) hello rozhraní příkazového řádku Azure. tooconfigure LAD pro nasazení classic modelu virtuálních počítačů (ASM), přepínače příliš "asm" režimu (`azure config mode asm`) a název skupiny prostředků hello v příkazu hello vynechat. Další informace najdete v tématu hello [dokumentaci rozhraní příkazového řádku a platformy](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Příklad LAD 3.0 konfigurace

Podle předchozích definice, zde je ukázka konfigurace rozšíření LAD 3.0 s vysvětlení hello. tooapply případě tohohle vzorku tooyour, by měli používat svůj vlastní název účtu úložiště, účet tokenu SAS a tokeny EventHubs SAS.

### <a name="privateconfigjson"></a>PrivateConfig.json

Tato nastavení privátní konfigurace:

* účet úložiště
* odpovídající tokenu SAS účtu
* několik jímky (JsonBlob nebo EventHubs se tokeny SAS)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

Tato nastavení veřejných způsobit LAD na:

* Nahrát procentuální hodnoty času procesoru a použitého místa disku metriky toohello `WADMetrics*` tabulky
* Odeslání zprávy syslog budovy "user" a závažnost "informace" toohello `LinuxSyslog*` tabulky
* Nahrát nezpracovaná OMI dotazu výsledky (PercentProcessorTime a PercentIdleTime) toohello s názvem `LinuxCPU` tabulky
* Nahrát připojením řádků v souboru `/var/log/myladtestlog` toohello `MyLadTestLog` tabulky

Data jsou v každém případě také odeslána a:

* Azure Blob storage (název kontejneru je definovaný v hello JsonBlob jímky)
* Koncový bod EventHubs (jak je uvedeno v hello EventHubs jímky)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

Hello `resourceId` v hello konfigurace shodovat, že z škálování virtuálních počítačů virtuálního počítače nebo hello hello nastavená.

* Metriky platformy Azure grafů a výstrah zná hello resourceId Dobrý den pracujete na virtuální počítač. Se očekává, že toofind hello data pro virtuální počítač pomocí hello resourceId hello vyhledávání klíče.
* Pokud používáte Azure škálování, musí odpovídat hello resourceId v konfiguraci automatického škálování hello používá LAD resourceId hello.
* Hello resourceId je integrovaná do hello názvy JsonBlobs zapsaných správcem LAD.

## <a name="view-your-data"></a>Zobrazení dat

Informace o výkonu Azure portálu tooview hello nebo nastavit výstrahy:

![Bitové kopie](./media/diagnostic-extension/graph_metrics.png)

Hello `performanceCounters` dat byly vždy uloženy v tabulce Azure Storage. Rozhraní API Azure úložiště jsou k dispozici pro mnoho jazyky a platformy.

Data odeslaná jímky tooJsonBlob se ukládají do objektů BLOB v účtu úložiště hello s názvem v hello [chráněné nastavení](#protected-settings). Můžete využívat data objektu blob hello pomocí všechny rozhraní API úložiště objektů Blob Azure.

Kromě toho můžete tyto uživatelského rozhraní nástroje tooaccess hello dat ve službě Azure Storage:

* Průzkumníka serveru Visual Studio.
* [Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").

Tento snímek relaci Microsoft Azure Storage Explorer zobrazuje hello vygenerovat tabulky služby Azure Storage a kontejnery z správně nakonfigurována rozšíření LAD 3.0 na testovací virtuální počítač. Obrázek Hello neodpovídá přesně hello [Ukázková konfigurace LAD 3.0](#an-example-lad-30-configuration).

![Bitové kopie](./media/diagnostic-extension/stg_explorer.png)

V tématu hello relevantní [EventHubs dokumentace](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn jak tooconsume zprávy publikované tooan EventHubs koncový bod.

## <a name="next-steps"></a>Další kroky

* Vytvoření metriky výstrahy v [Azure monitorování](../../monitoring-and-diagnostics/insights-alerts-portal.md) pro shromažďování metrik hello.
* Vytvoření [tabulek sledování](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) pro vaše metriky.
* Zjistěte, jak příliš[vytvořit škálovací sadu virtuálních počítačů](/azure/virtual-machines/linux/tutorial-create-vmss) pomocí vaší automatické škálování toocontrol metriky.
