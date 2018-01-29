---
title: "Jazyky pro virtuální počítač vědecké účely dat v Azure | Microsoft Docs"
description: "Jazyky pro datové vědy virtuálního počítače na Azure"
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
ms.openlocfilehash: 2f2125e739b738847e03ce429d65801969611685
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Jazyky podporované v dat virtuálního počítače vědecké účely 

Virtuální počítač vědecké účely dat (DSVM) obsahuje několik předdefinovaných jazyky a nástroje pro vývoj pro vytváření aplikací AI. Zde jsou některé z nejdůležitějšími ty, které jsou. 

## <a name="python"></a>Python

|    |           |
| ------------- | ------------- |
| Podporované jazykové verze | 2.7 a 3.5 |
| Podporované DSVM edice      | Linux, Windows     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Dva globální `conda` prostředí jsou vytvořeny. <br /> * `root`prostředí nacházející se v `/anaconda/` je Python 2.7. <br/> * `py35`prostředí nacházející se v `/anaconda/envs/py35`je Python 3.5       |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter pro jazyk Python jsou zahrnuty     |
| Na DSVM souvisejících nástrojích      | Dita PySpark, R,      |
### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?    

**Windows**:

* Spouštění v příkazovém řádku

Otevřete příkazový řádek a udělejte v závislosti na verzi jazyka Python, kterou chcete spustit. 

```
# To run Python 2.7
activate 
python --version

# To run Python 3.5
activate py35
python --version

```
* Použití v IDE

Použití nástrojů Python pro Visual Studio (PTVS) nainstalované v edici Visual Studio Community. Instalace pouze prostředí automaticky v PTVS v Python 2.7. 
> [!NOTE]
> Chcete-li bodu PTVS Python 3.5, vytvořte vlastní prostředí v PTVS. Chcete-li nastavit této cesty prostředí Visual Studio Community Edition, přejděte na **nástroje** -> **Python Tools** -> **prostředí Python** a pak klikněte na **+ vlastní**. Potom nastavte jeho umístění na `c:\anaconda\envs\py35` a pak klikněte na _automatické rozpoznání_. 

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako _Python [Conda kořenové]_ pro Python 2.7 a _Python [Conda env:py35]_ pro prostředí Python 3.5. 

* Instalaci balíčků Python

Prostředí Python výchozí na DSVM jsou globální prostředí čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, aktivovat do kořenového adresáře nebo py35 prostředí pomocí `activate` příkaz jako správce. Pak můžete použít Správce balíčků jako `conda` nebo `pip` instalaci nebo aktualizaci balíčků. 


**Linux**:

* Spouštění v terminálu

Otevřete terminál a udělejte v závislosti na verzi jazyka Python, kterou chcete spustit. 

```
# To run Python 2.7
source activate 
python --version

# To run Python 3.5
source activate py35
python --version

```
* Použití v IDE

Použijte PyCharm nainstalovaný ve Visual Studio Community edition. 

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako _Python [Conda kořenové]_ pro Python 2.7 a _Python [Conda env:py35]_ pro prostředí Python 3.5. 

* Instalaci balíčků Python

Prostředí Python výchozí na DSVM jsou globální prostředí čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, aktivovat do kořenového adresáře nebo py35 prostředí pomocí `source activate` příkaz jako správce nebo uživatel s oprávněními sudo. Pak můžete použít Správce balíčků jako `conda` nebo `pip` instalaci nebo aktualizaci balíčků. 

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Podporované jazykové verze | Microsoft R otevřete 3.x (100 % kompatibilní s CRAN r.<br /> Microsoft R Server 9.x Developer edition (Enterprise škálovatelné A připravena platforma R)|
| Podporované DSVM edice      | Linux, Windows     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Windows:`C:\Program Files\Microsoft\R Server\R_SERVER` <br />Linux:` /usr/lib64/microsoft-r/3.3/lib64/R`    |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter pro R jsou zahrnuty     |
| Na DSVM souvisejících nástrojích      | Dita SparkR, Python,      |
### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?    

**Windows**:

* Spouštění v příkazovém řádku

Otevřete příkazový řádek a zadejte právě `R`.

* Použití v IDE

Použití RTools pro Visual Studio (RTVS) nainstalovaný ve Visual Studio Community edition nebo Rstudia. Tyto jsou k dispozici v nabídce start nebo jako ikony na ploše. 

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako _R_ použít Jupyter R jádra (IRKernel). 

* Instalace balíčků R

R je nainstalováno na DSVM v prostředí globální čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, spusťte R pomocí jedné z výše uvedených metod. Potom můžete spustit Správce balíčků R `install.packages()` instalaci nebo aktualizaci balíčků. 

**Linux**:

* Spouštění v terminálu

Otevřete terminálu a právě spustit `R`.  

* Použití v IDE

Použijte Rstudia nainstalovaná na Linux DSVM.  

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako _R_ použít Jupyter R jádra (IRKernel). 

* Instalace balíčků R

R je nainstalováno na DSVM v prostředí globální čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, spusťte R pomocí jedné z výše uvedených metod. Potom můžete spustit Správce balíčků R `install.packages()` instalaci nebo aktualizaci balíčků. 


## <a name="julia"></a>Dita

|    |           |
| ------------- | ------------- |
| Podporované jazykové verze | 0.5 |
| Podporované DSVM edice      | Linux, Windows     |
| Jak je ho nakonfigurovaná a nainstalovaná na DSVM?  | Systém Windows: V nainstalován`C:\JuliaPro-VERSION`<br /> Linux: Nainstalovaným v`/opt/JuliaPro-VERSION`    |
| Odkazy na ukázky      | Ukázka poznámkové bloky Jupyter pro Dita jsou zahrnuty     |
| Na DSVM souvisejících nástrojích      | Python, R      |
### <a name="how-to-use--run-it"></a>Jak se použít nebo ji spustit?    

**Windows**:

* Spouštění v příkazovém řádku

Otevřete příkazový řádek a stačí spustit `julia`. 
* Použití v IDE

Použití `Juno` Dita IDE nainstalovaná na DSVM a k dispozici jako zástupce na ploše.

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako`Julia VERSION` 

* Instalace balíčků Dita

Výchozí umístění Dita je globální prostředí čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, spusťte Dita pomocí jedné z výše uvedených metod. Potom můžete spustit balíček Dita manager příkazy, jako je `Pkg.add()` instalaci nebo aktualizaci balíčků. 


**Linux**:
* Je spuštěna v terminálu.

Otevřete terminálu a právě spustit `julia`. 
* Použití v IDE

Použití `Juno` Dita IDE nainstalovaná na DSVM a k dispozici jako zástupce nabídce aplikace.

* Použití v Jupyter

Otevřete Jupyter a klikněte na `New` tlačítko vytvořte nový poznámkový blok. V tomto okamžiku můžete si zvolit typ jádra jako`Julia VERSION` 

* Instalace balíčků Dita

Výchozí umístění Dita je globální prostředí čitelný všichni uživatelé. Ale pouze správci mohou zápisu / globální balíčky nainstalovat. Chcete-li nainstalovat balíček do globální prostředí, spusťte Dita pomocí jedné z výše uvedených metod. Potom můžete spustit balíček Dita manager příkazy, jako je `Pkg.add()` instalaci nebo aktualizaci balíčků. 

## <a name="other-languages"></a>Ostatní jazyky

**C#**: k dispozici v systému Windows a přístupné prostřednictvím Visual Studio Community edition nebo na `Developer Command Prompt for Visual Studio` kde můžete jenom spouštět `csc` příkaz. 

**Java**: OpenJDK je k dispozici v systému Linux a Windows edice DSVM a sady v cestě. Můžete zadat `javac` nebo `java` příkazu na příkazovém řádku v systému Windows nebo na prostředí bash v systému Linux používat Java. 

**Node.js**: node.js je k dispozici v systému Linux a Windows edice DSVM a sady v cestě. Můžete zadat `node` nebo `npm` příkazu na příkazovém řádku v systému Windows nebo na prostředí bash v systému Linux pro přístup k node.js. V systému Windows je nainstalován nástroje Node.js pro rozšíření sady Visual Studio, který umožní grafického rozhraní IDE pro vývoj aplikace node.js. 

**F #**: k dispozici v systému Windows a přístupné prostřednictvím Visual Studio Community edition nebo na `Developer Command Prompt for Visual Studio` kde můžete jenom spouštět `fsc` příkaz. 


