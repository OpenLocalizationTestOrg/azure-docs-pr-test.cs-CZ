---
title: "aaaRun Azure Batch úlohy začátku do konce bez nutnosti psaní kódu (Preview) | Microsoft Docs"
description: "Vytvoření šablony souborů pro hello rozhraní příkazového řádku Azure toocreate Batch fondy, úlohy a úkoly."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.openlocfilehash: c0434d09766451f6ba516efbad949834711ee819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)

Pomocí hello rozhraní příkazového řádku Azure je možné toorun dávkových úloh bez nutnosti psaní kódu.

Soubory šablon můžete vytvořit a použít s hello rozhraní příkazového řádku Azure, umožňující fondy, úlohy a úlohy toobe vytvořit dávky. Vstupní soubory úlohy můžete snadno nahrán do účtu úložiště hello přidruženého k hello dávkového účtu a úlohy výstupní soubory stáhnout.

## <a name="overview"></a>Přehled

Rozšíření toohello rozhraní příkazového řádku Azure umožňuje Batch toobe používá k kompletní uživatelé, kteří nejsou vývojáři. Fond lze vytvořit, nahrán vstupních dat, úlohy a přidružených úloh vytvoření a hello výsledný výstup stažených dat – žádný kód potřeby hello rozhraní příkazového řádku používá přímo, nebo se integrovaný do skriptů.

Vytvoření šablony batch ve hello [existující podporu Batch v hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) umožňuje soubory JSON toospecify hodnoty vlastností pro vytvoření hello fondy, úlohy, úlohy a další položky. S šablonami Batch hello následující funkce byly přidány prostřednictvím co je možné pomocí soubory JSON hello:

-   Parametry lze definovat. Když se použije šablona hello, jsou pouze hodnoty parametrů hello zadaný toocreate hello položku se dalších hodnot vlastností položky specifikované v textu hello šablony. Uživatel, který jste srozuměni s tím, že Batch a toobe aplikace spouštěné dávky můžete vytvořit šablony, fond, úlohy a hodnoty vlastností úlohy. Uživatel méně neznáte Batch nebo aplikace jenom potřebuje toospecify hello hodnoty pro parametry definované hello.

-   Vytvořit objekty pro vytváření úloh úlohy nebo další úkoly spojené s úlohou, zabraňující hello nutné pro mnoho toobe definice úloh vytvoření a výrazně zjednodušení odeslání úlohy.


Vstupní datové soubory potřebovat toobe zadaná pro úlohy a často vytváří výstupní datové soubory. Je přidružený účet úložiště, ve výchozím nastavení, ke každé dávky účet a soubory lze snadno přenášená tooand z tohoto účtu úložiště pomocí rozhraní příkazového řádku bez kódování a vyžadují přihlašovací údaje žádné úložiště.

Například [ffmpeg](http://ffmpeg.org/) je Oblíbené aplikace, která zpracovává soubory audia a videa. Hello rozhraní příkazového řádku Azure Batch se dá použít tooinvoke ffmpeg tootranscode zdroj video soubory toodifferent řešení.

-   Je-li vytvořit šablonu fondu. uživatel Hello vytváření šablony hello zná, jak toocall hello ffmpeg aplikace a požadavky na jeho; Určí hello odpovídající operačního systému, počítač velikost, jak ffmpeg je nainstalovaná (z balíčku aplikace nebo pomocí Správce balíčků, například) a dalších fond hodnoty vlastností. Parametry jsou vytvořeny, takže pokud se použije šablona hello, pouze id fondu hello a počet virtuálních počítačů musí být toobe zadán.

-   Je-li vytvořit šablonu úlohy. Šablona vytváření hello Hello uživatel zná jak ffmpeg potřebám toobe vyvolány tootranscode zdroj videa tooa jiné rozlišení a určuje příkazový řádek úkolu hello; také věděli, že je složka obsahující hello zdrojové video soubory, se úloha vyžaduje pro vstupní soubor.

-   Koncový uživatel se sadou tootranscode video soubory nejprve vytvoří fond pomocí šablony hello fondu, zadáte jenom id fondu hello a počet virtuálních počítačů, které jsou potřeba. Potom můžete nahrát hello zdrojové soubory tootranscode. Úlohy lze pak odeslat pomocí šablony úlohy hello, zadáte jenom id fondu hello a umístění zdrojových souborů hello nahrát. jeden úkol za vstupní soubor generován se vytvoří Hello dávkovou úlohu. Nakonec hello převodu výstupní soubory lze stáhnout.

## <a name="installation"></a>Instalace

Možnosti Hello šablonu a soubor přenosu vyžadují rozšíření toobe nainstalována.

Pokyny, jak zjistit, tooinstall hello rozhraní příkazového řádku Azure [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

Jednou hello byla nainstalována rozhraní příkazového řádku Azure, hello Batch rozšíření můžete nainstalovat pomocí rozhraní příkazového řádku následující příkaz:

```azurecli
az component update --add batch-extensions --allow-third-party
```

Další informace o hello Batch rozšíření najdete v tématu [Microsoft Azure Batch CLI rozšíření pro Windows, Mac a Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).

## <a name="templates"></a>Šablony

Hello rozhraní příkazového řádku Azure Batch umožňuje položek, jako jsou fondy, úlohy a úlohy toobe vytvořit zadáním soubor JSON obsahující názvy a hodnoty vlastností. Například:

```azurecli
az batch pool create –-json-file AppPool.json
```

Šablony Azure Batch jsou podobné tooAzure šablony Resource Manageru, v syntaxi a funkce. Jsou soubory JSON, které obsahují položky názvy a hodnoty vlastností, ale přidat hello následující hlavní koncepty:

-   **Parametry**

    -   Povolí vlastnost toobe hodnoty zadané v části textu, s pouze hodnoty parametrů toobe zadán, pokud je hello šablony, která potřebuje. Například hello dokončení definice pro fond může být umístěny v textu hello a jenom jeden parametr definovaný pro id fondu; pouze řetězec id fondu proto musí toocreate toobe zadaný fond.
        
    -   Šablona textu Hello můžete vytvořené uživatelem s znalosti Batch a toobe aplikace hello spusťte dávkou; Pokud je hello šablony je nutné zadat pouze hodnoty pro parametry definované Autor hello. Uživatel bez hello podrobný Batch nebo znalostní bázi aplikace lze tedy použít šablony.

-   **Proměnné**

    -   Povolit parametr jednoduché nebo komplexní hodnoty toobe zadaný v jednom místě a použít na jeden nebo více místech v textu hello šablony. Proměnné můžete zjednodušit a zmenšit velikost hello hello šablony, a také zkontrolujte více udržovatelný tak, že jeden vlastnosti toochange umístění, jehož hodnota může změnit.

-   **Konstrukce vyšší úrovně**

    -   Některé konstrukce vyšší úrovně jsou k dispozici v hello šablony, které ještě nejsou k dispozici v hello rozhraní API služby Batch. Například objekt pro vytváření úloh lze definovat v šabloně úlohy, která vytvoří více úloh pro úlohu hello pomocí společná definice úlohy. Tyto konstrukce brání hello nutné toocode dynamicky vytvořte několik souborů JSON, jako je například jeden soubor pro úlohy, a také soubory tooinstall aplikací prostřednictvím Správce balíčků, například vytvořit skript.

    -   V některých bodu, kde použít, že může být tyto konstrukce přidat toothe Batch služby a k dispozici v hello rozhraní API služby Batch, uživatelská atd.

### <a name="pool-templates"></a>Šablony fondu

Kromě toho hello fondu šablony podporuje možnosti standardní šablona toohello parametry a proměnné, hello následující konstrukce vyšší úrovně:

-   **Odkazy na balíček**

    -   Volitelně umožňuje uzly toopool toobe zkopírovat softwaru pomocí balíčku správci. nejsou zadány Hello package manager a id balíčku. Je možné toodeclare jeden nebo více balíčků zabraňuje hello nutné toocreate skript, který získá hello požadované balíčky, nainstalujete hello skript a spusťte skript hello na všech uzlech fondu.

Následující Hello je příkladem šablonu, která vytvoří fond virtuálních počítačů Linux s ffmpeg nainstalovaná a pouze vyžaduje fondu id řetězec a hello počet toouse toobe zadaný virtuální počítače:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "hello number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Pokud se název souboru šablony hello _fondu ffmpeg.json_, pak hello šablony by být volána následujícím způsobem:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a>Šablony úloh

Kromě toho šablony úlohy hello podporuje možnosti standardní šablona toohello parametry a proměnné, hello následující konstrukce vyšší úrovně:

-   **Objekt pro vytváření úloh**

    -   Vytvoří více úkolů pro úlohu z definice jeden úkol. Tři typy objektu pro vytváření úloh jsou podporovány – čištění oblouku úloh na soubor a kolekce úloh.

Hello následuje příklad šablony, která vytvoří úlohu, která používá ffmpeg k tooone video soubory MP4 převod z dvě nižší rozlišení, s vytvořenými pro jednotlivé zdroje videosoubor úloh:

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch pool which runs hello job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Pokud se název souboru šablony hello _úlohy ffmpeg.json_, pak hello šablony by být volána následujícím způsobem:

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a>Skupiny souborů a přenos souborů

Většina úloh a úloh vyžadují vstupní soubory a vytvoří výstupní soubory. Obě vstupní soubory a výstupní soubory obvykle nutné toobe přenést z uzlu toohello hello klienta nebo z klienta toohello hello uzlu. Hello rozšíření rozhraní příkazového řádku Azure Batch abstrahuje přenos nepřítomnosti souborů a využívá hello účet úložiště, který se vytvoří ve výchozím nastavení pro každý účet Batch.

Skupinu souborů znamená zároveň tooa kontejneru, který je vytvořen v hello účtu úložiště Azure. Skupina souborů Hello může mít podsložky.

Hello rozšíření rozhraní příkazového řádku Batch poskytuje příkazy pro odesílání souborů ze skupiny klientů tooa zadaný soubor a stahování souborů z hello zadaný soubor skupiny tooa klienta.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Fond a úlohy šablony povolí soubory uložené v souboru skupiny toobe zadané pro kopírování na uzly fondu nebo vypnout fond uzlů zpět tooa skupiny souborů. Například v šabloně úlohy zadali dřív hello souboru skupiny "ffmpeg – vstup" je zadán pro vytváření úloh hello jako hello umístění hello zdrojových souborů video zkopírovat dolů na ni hello uzel pro překódování; Hello souboru skupiny "ffmpeg-output" se používá jako hello umístění, kde se hello převodu výstupní soubory zkopírovat toofrom hello uzlu se systémem každý úkol.

## <a name="summary"></a>Souhrn

Podpora přenos šablonu a soubor aktuálně byly přidány pouze toohello rozhraní příkazového řádku Azure. cílem Hello je cílová skupina hello tooexpand, který můžete použít toousers Batch, který není nutné toodevelop kódu pomocí hello rozhraní API služby Batch, například výzkumných pracovníků, IT uživatelé a tak dále. Bez kódování, uživatelům se znalostí Azure Batch a hello toobe aplikace spouštěné dávky můžete vytvořit šablony pro vytvoření fondu a úlohy. Parametry šablony uživatelé bez podrobné údaje o Batch a hello aplikace pomocí šablony hello.

Vyzkoušet hello Batch rozšíření pro hello rozhraní příkazového řádku Azure a poskytnout nám žádné zpětnou vazbu nebo návrhy, buď v hello komentáře k tomuto článku nebo prostřednictvím hello [fórum Azure Batch](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).

## <a name="next-steps"></a>Další kroky

- Najdete v příspěvku blogu pro šablony hello Batch: [hello úloh systémem Azure Batch pomocí rozhraní příkazového řádku Azure – vyžaduje žádný kód](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).
- Podrobnou dokumentaci instalaci a použití, ukázky a zdrojového kódu jsou k dispozici v hello [úložiště Azure GitHub](https://github.com/Azure/azure-batch-cli-extensions).
