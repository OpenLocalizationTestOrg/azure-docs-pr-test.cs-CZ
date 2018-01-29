---
title: "Hloubkové učení a AI architektury - Azure | Microsoft Docs"
description: "Architektura AI a hloubkového učení"
keywords: "datové vědy nástroje, datové vědy virtuálního počítače, nástroje pro vědecké zpracování dat, vědecké zpracování dat linux"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: c6dba433536e07970ad5ce338e33b5d19f4532b2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Architektura AI a hloubkového učení
[Datové vědy virtuálního počítače](http://aka.ms/dsvm) (DSVM) a [počítač hloubkového učení](http://aka.ms/dsvm/deeplearning) podporuje mnoho hloubkové learning rozhraní k sestavení aplikace umělé Intelligence (AI) s prediktivní analýzy a kognitivní možnosti jako bitovou kopii a znalosti jazyka. 

Zde jsou uvedeny podrobnosti na všechny hloubkového učení rozhraní, které jsou k dispozici na DSVM.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Microsoft kognitivní Toolkit (CNTK) je nainstalována v Python 2.7 v _kořenové_ prostředí, stejně jako Python 3.5 v _py35_ prostředí.   |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter jsou zahrnuty.     |
| Na DSVM souvisejících nástrojích      | Keras      |
| Jak se použít nebo ji spustit?    | Otevřít Jupyter a pak vyhledejte složku CNTK  |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | V systému Linux, je nainstalován TensorFlow v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí. V systému Windows, je nainstalován Tensorflow v Python 3.5 (_py35_) prostředí.  |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter jsou zahrnuty.     |
| Na DSVM souvisejících nástrojích      | Keras      |
| Jak se použít nebo ji spustit?    | Otevřete Jupyter a potom vyhledejte složku TensorFlow.  |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Keras je nainstalován v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí.   |
| Odkazy na ukázky      | https://github.com/fchollet/keras/tree/master/examples      |
| Na DSVM souvisejících nástrojích      | Microsoft kognitivní Toolkit, TensorFlow, Theano      |
| Jak se použít nebo ji spustit?    | Stáhnout ukázky z Githubu umístění, zkopírujte jej do adresáře v rámci ~/notebooks a otevře ji v Jupyter   |




## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Je nainstalovaný Caffe `/opt/caffe`.    |
| Odkazy na ukázky      | Ukázky jsou součástí `/opt/caffe/examples`.      |
| Na DSVM souvisejících nástrojích      | Caffe2      |
### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?  

Přihlaste se k virtuálnímu počítači, pak spusťte nový terminál a zadat pomocí X2Go

```
cd /opt/caffe/examples
jupyter notebook
```

Otevře se nové okno prohlížeče s ukázka poznámkových bloků.

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Je nainstalovaný Caffe2 `/opt/caffe2`. Je také k dispozici pro Python 2.7 (_kořenové_) conda prostředí.     |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter jsou zahrnuty     |
| Na DSVM souvisejících nástrojích      | Caffe      |
| Jak se použít nebo ji spustit?    | Otevřít Jupyter a pak přejděte do adresáře Caffe2 najít ukázkové poznámkových bloků. Některé poznámkových bloků vyžadují kořenu Caffe2 ve kód Python; Zadejte /opt/caffe2.   |


## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Zřetězeného souboru je nainstalován v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí. ChainerRL a ChainerCV jsou také nainstalované.   |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter jsou zahrnuty.      |
| Na DSVM souvisejících nástrojích      | Caffe      |

### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?  

V terminálu, aktivovat verzi jazyka Python chcete (_kořenové_ nebo _py35_) spusťte _python_, pak importovat zřetězeného souboru. V Jupyter vyberte Python 2.7 nebo 3.5 jádra a pak importovat zřetězeného souboru.


## <a name="deep-water"></a>Hloubkové horních

|    |           |
| ------------- | ------------- |
| Co je to?   | Architektura hloubkové learning pro H2O      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Hloubkové horních je nainstalován v `/dsvm/tools/deep_water`.   |
| Odkazy na ukázky      | Ukázky jsou dostupné prostřednictvím hloubkového horních serveru.      |
| Na DSVM souvisejících nástrojích      | H2O, šumivého horních      |

### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?  

Připojte k virtuálnímu počítači pomocí X2Go. V terminálu spuštění hloubkového horních serveru:

    java -jar /dsvm/tools/deep_water/h2o.jar

Potom spusťte prohlížeč a připojení k `http://localhost:54321`.



## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Je nainstalovaný MXNet `C:\dsvm\tools\mxnet` v systému Windows a `/dsvm/tools/mxnet` v systému Linux. Python vazby jsou nainstalovány v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí. Vazby R jsou také nainstalované.   |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter jsou zahrnuty.    |
| Na DSVM souvisejících nástrojích      | Keras      |
| Jak se použít nebo ji spustit?    | Otevřít Jupyter a pak vyhledejte složku mxnet  |

## <a name="nvidia-digits"></a>NVIDIA ČÍSLIC

|    |           |
| ------------- | ------------- |
| Co je to?   | Přímý učení systému z NVIDIA pro rychlé cvičení hloubkové learning modely      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | ČÍSLIC je nainstalován v `/dsvm/tools/DIGITS` a je k dispozici služba byla požádána _číslic_.   |
### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?  

Přihlaste se k virtuálnímu počítači pomocí X2Go. V terminálu spusťte službu:

    sudo systemctl start digits

Službu spusťte trvá přibližně jednu minutu. Spustit webový prohlížeč a přejděte do `http://localhost:5000`.



## <a name="nvdia-smi"></a>nvdia-smi

|    |           |
| ------------- | ------------- |
| Co je to?   | NVIDIA nástroj pro dotazování aktivita GPU      |
| Podporované DSVM edice      | Windows, Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | _NVIDIA smi_ je k dispozici na cestě k systému souborů.   |
| Jak se použít nebo ji spustit? | Spustit příkazový řádek (v systému Windows) nebo terminál (v systému Linux), a poté spusťte _nvidia smi_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Theano je nainstalován v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí.   |
| Na DSVM souvisejících nástrojích      | Keras      |
| Jak se použít nebo ji spustit?    | V terminálu aktivujte verzi Pythonu (kořenové nebo py35), spusťte python a pak importovat theano. V Jupyter vyberte Python 2.7 nebo 3.5 jádra a pak importovat theano.  |



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Co je to?   | Hloubkové learning framework      |
| Podporované DSVM edice      | Linux     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Je nainstalovaný svítilnou `/dsvm/tools/torch`. PyTorch je nainstalován v Python 2.7 (_kořenové_), a také Python 3.5 (_py35_) prostředí.   |
| Odkazy na ukázky      | Ukázky svítilnou jsou umístěné na `/dsvm/samples/torch`. Ukázky PyTorch jsou umístěné na `/dsvm/samples/pytorch`.      |

