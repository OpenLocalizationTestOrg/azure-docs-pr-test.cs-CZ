---
title: "AAA \"Vytvoření události fondu Azure Batch | Microsoft Docs\""
description: "Referenční dokumentace pro fondu Batch vytvoření události."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: ad31251969af553baa21e8c533d8ea95d3eeaf91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-create-event"></a>Vytvoření fondu události

 Tato událost je vygenerované po vytvoření fondu. obsah Hello hello protokolu zveřejní obecné informace o fondu hello. Všimněte si, že pokud hello cílovou velikost fondu hello je větší než 0 výpočetní uzly, bude následovat počáteční událost změny velikosti fondu ihned po této události.

 Hello následující příklad ukazuje textu hello fondu vytvoření události pro fond vytvořený hello CloudServiceConfiguration vlastnost.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Hello id fondu hello.|
|displayName|Řetězec|Hello zobrazovaný název fondu hello.|
|vmSize|Řetězec|velikost Hello hello virtuální počítače ve fondu hello. Všechny virtuální počítače ve fondu jsou hello stejnou velikost. <br/><br/> Informace o dostupných velikostí virtuálních počítačů pro Cloud Services najdete v části fondy (fondy vytvořené pomocí cloudServiceConfiguration), [velikosti pro cloudové služby](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch podporuje všechny velikosti virtuálních počítačů služby Cloud s výjimkou `ExtraSmall`.<br/><br/> Informace o dostupných virtuálních počítačů najdete v části velikosti pro fondy pomocí bitové kopie z hello Marketplace virtuálních počítačů (fondy vytvořené pomocí virtualMachineConfiguration) [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) nebo [velikostí pro Virtuální počítače](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Služba Batch podporuje všechny velikosti VM Azure kromě `STANDARD_A0` a těch, které mají úložiště Premium (série `STANDARD_GS`, `STANDARD_DS` a `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Komplexní typ|Konfigurace služby Hello cloudu pro fond hello.|
|[virtualMachineConfiguration](#bk_vmconf)|Komplexní typ|Konfigurace virtuálního počítače Hello hello fondu.|
|[networkConfiguration](#bk_netconf)|Komplexní typ|Konfigurace sítě Hello hello fondu.|
|resizeTimeout|Čas|Hello časový limit pro přidělení výpočetní uzly toohello fondu zadané pro hello poslední operace změny velikosti ve fondu hello.  (hello počáteční dimenzování při vytvoření fondu hello počty jako změny velikosti.)|
|targetDedicated|Int32|Hello počet výpočetních uzlů, které jsou požadovány pro fond hello.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu hello automaticky přizpůsobí v čase.|
|enableInterNodeCommunication|BOOL|Určuje, zda fond hello je nastaven pro přímou komunikaci mezi uzly.|
|isAutoPool|BOOL|Speficies jestli hello fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.|
|maxTasksPerNode|Int32|Hello maximální počet úloh, které můžou běžet současně na jednom výpočetním uzlu ve fondu hello.|
|vmFillType|Řetězec|Definuje, jak hello služba Batch distribuuje úkoly mezi výpočetní uzly ve fondu hello. Platné hodnoty jsou rozloženy nebo aktualizací Service Pack.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|atribut osFamily|Řetězec|Hello Azure hostovaného operačního systému rodiny toobe nainstalovat na hello virtuální počítače ve fondu hello.<br /><br /> Možné hodnoty:<br /><br /> **2** – operační systém řady 2, ekvivalentní tooWindows Server 2008 R2 SP1.<br /><br /> **3** – operačního systému rodiny 3, ekvivalentní tooWindows Server 2012.<br /><br /> **4** – 4 rodiny operačního systému, ekvivalentní tooWindows Server 2012 R2.<br /><br /> Další informace najdete v tématu [verzí hostovaného operačního systému Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Řetězec|verze toobe Hello Azure hostovaného operačního systému nainstalovat na hello virtuální počítače ve fondu hello.<br /><br /> Hello výchozí hodnota je  **\***  který určuje hello nejnovější verze operačního systému pro hello zadaný rodiny.<br /><br /> Pro ostatní povolené hodnoty, najdete v části [verzí hostovaného operačního systému Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|[Element imageReference](#bk_imgref)|Komplexní typ|Určuje informace o hello platformy nebo Marketplace image toouse.|
|nodeAgentSKUId|Řetězec|Hello SKU agenta uzlu Batch hello zřízený hello výpočetním uzlu.|
|[windowsConfiguration](#bk_winconf)|Komplexní typ|Určuje nastavení operačního systému Windows hello virtuálního počítače. Tato vlastnost nesmí být zadán, pokud element imageReference hello odkazuje na bitovou kopii operačního systému Linux.|

###  <a name="bk_imgref"></a>Element imageReference

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|Vydavatele|Řetězec|Vydavatel Hello hello bitové kopie.|
|Nabídka|Řetězec|Nabídka Hello hello bitové kopie.|
|SKU|Řetězec|Hello SKU hello bitové kopie.|
|Verze|Řetězec|verze Hello hello bitové kopie.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|enableAutomaticUpdates|Logická hodnota|Určuje, zda text hello virtuální počítač povolena pro automatické aktualizace. Pokud není tato vlastnost určena, hello výchozí hodnota je true.|

###  <a name="bk_netconf"></a>networkConfiguration

|Název elementu|Typ|Poznámky|
|------------------|--------------|----------|
|subnetId|Řetězec|Určuje identifikátor zdroje hello hello podsítě, ve které hello fondu výpočetní uzly jsou vytvořeny.|
