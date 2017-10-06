---
title: "aaaWorking s modulů Node.js"
description: "Zjistěte, jak toowork s modulů Node.js při použití služby Azure App Service nebo cloudové služby."
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 926358b7eb80a659dbc1015686b06a30d8c9b8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Používání modulů Node.js s aplikacemi Azure
Tento dokument obsahuje pokyny k používání modulů Node.js s aplikacemi, které jsou hostované v Azure. Poskytuje rady, jak zajistit, aby aplikace používala konkrétní verzi modulu, a jak používat nativní moduly s Azure.

Pokud jste již obeznámeni s použitím modulů Node.js **package.json** a **npm shrinkwrap.json** soubory, hello následující informace poskytuje rychlý přehled o co je popsané v tomto článku:

* Aplikační služba Azure rozumí **package.json** a **npm shrinkwrap.json** soubory a můžete nainstalovat moduly založené na položky v těchto souborech.

* Azure Cloud Services očekávat všechny moduly toobe nainstalované na hello vývojového prostředí a hello **uzlu\_moduly** toobe adresáře, které jsou součástí balíčku pro nasazení hello. Je možné tooenable podpora pro instalaci modulů pomocí **package.json** nebo **npm shrinkwrap.json** soubory na cloudových služeb, ale tato konfigurace vyžaduje přizpůsobení hello výchozí skripty, které používá projekty cloudových služeb. Pro příklad tooconfigure tohoto prostředí najdete v části [Azure spuštění úloh toorun npm nainstalujte tooavoid nasazení uzlu moduly](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Virtuální počítače Azure nejsou popsané v tomto článku jako hello možnosti nasazení do virtuálního počítače, je závislá na hello operačního systému hostované hello virtuálního počítače.
> 
> 

## <a name="nodejs-modules"></a>Modulů Node.js
Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci. Moduly jsou obvykle nainstalován pomocí hello **npm** příkazového řádku nástroje, ale jsou uvedeny některé moduly (jako je například modul http hello) jako součást balíčku Node.js hello jádra.

Při instalaci moduly jsou uložené v hello **uzlu\_moduly** adresář na hello kořenového adresáře aplikace adresářové struktury. Každý modul v rámci hello **uzlu\_moduly** directory udržuje vlastní **uzlu\_moduly** adresář, který obsahuje všechny moduly, které závisí na a toto chování se opakuje pro každý modul všechny hello směrem dolů řetězec závislostí hello. Toto prostředí umožňuje každý modul nainstalovaný toohave požadavky svou vlastní verzi hello moduly, že závisí na, ale může vést k poměrně velké adresářové struktury.

Nasazení hello **uzlu\_moduly** adresáři jako součást aplikace zvyšuje velikost hello hello nasazení srovnání toousing **package.json** nebo  **npm shrinkwrap.json** soubor; však zaručit, že hello verzích hello moduly používané v produkčním prostředí jsou text hello stejné jako hello moduly používané v vývoj.

### <a name="native-modules"></a>Nativní moduly
Většina moduly jsou soubory JavaScript jednoduše prostého textu, některé moduly jsou specifické pro platformu binární bitové kopie. Tyto moduly jsou kompilovány během instalace, obvykle pomocí Pythonu a gyp uzlu. Vzhledem k tomu, že Azure Cloud Services spoléhají na hello **uzlu\_moduly** složky, které jsou nasazované jako součást aplikace hello žádné nativní modul zahrnuty jako součást hello nainstalované moduly by měly fungovat v cloudové službě, dokud se nainstalovaná a kompilovat v systému Windows, vývoj.

Aplikační služba Azure nepodporuje všechny nativní moduly a může selhat, když kompilujete modulů s konkrétní požadavky. I když některé oblíbené moduly jako MongoDB mají volitelné nativní závislosti a bez problémů fungují bez nich, prokázat dvě řešení úspěšné s téměř všechny nativní moduly, které jsou k dispozici dnes:

* Spustit **npm nainstalujte** na počítači s Windows, který má všechny hello nativního modulu nainstalovány požadované součásti. Potom nasaďte hello vytvořit **uzlu\_moduly** složku jako součást tooAzure hello aplikace služby App Service.

  * Před kompilování, zkontrolujte, zda vaše místní instalace Node.js má odpovídající architektuře a verzi hello je co možná toohello používá v Azure (hello aktuální hodnoty lze zkontrolovat na runtime z vlastností **process.arch**a **process.version**).

* Aplikační služba Azure může být nakonfigurované tooexecute vlastní bash nebo skripty prostředí během nasazení, která poskytuje hello možnost tooexecute vlastní příkazy a přesněji nakonfigurovat hello způsob **npm nainstalujte** je spuštěn. Pro video znázorňující způsob tooconfigure prostředí, najdete v části [skriptů nasazení vlastní web s Kudu].

### <a name="using-a-packagejson-file"></a>Pomocí souboru package.json
Hello **package.json** soubor je způsob toospecify hello nejvyšší úrovně závislosti vaše aplikace vyžaduje, aby hello hostující platforma můžete nainstalovat hello závislosti, namísto nutnosti tooinclude hello **uzlu \_balíčky** složku jako součást nasazení hello. Po nasazení aplikace hello hello **npm nainstalujte** příkaz je použité tooparse hello **package.json** souboru a nainstalujte všechny uvedené závislosti hello.

Během vývoje, můžete použít hello **– Uložit**, **– uložit dev**, nebo **– uložit volitelné** parametry při instalaci modulů tooadd položka pro hello modulu tooyour **package.json** souboru automaticky. Další informace najdete v tématu [npm install](https://docs.npmjs.com/cli/install).

Jeden možný problém s hello **package.json** soubor je, že pouze určuje hello verze závislosti nejvyšší úrovně. Každý modul nainstalovaný může nebo nemusí zadejte verzi hello hello modulů, které závisí na, a proto je možné, že můžete dospět řetězem různých závislostí než hello používá v vývoj.

> [!NOTE]
> Pokud se nasazuje tooAzure služby App Service vaše <b>package.json</b> soubor odkazuje na nativní modul, může dojít k chybě podobné toohello následující ukázka při publikování aplikace hello pomocí Git:
> 
> npm ERR! module-name@0.6.0Nainstalujte: 'uzlu gyp konfigurace buildu.
> 
> npm ERR! ' cmd "/ c" "uzlu gyp konfigurace sestavení" ' se nezdařilo s 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Pomocí npm shrinkwrap.json souboru.
Hello **npm shrinkwrap.json** soubor je pokusu o tooaddress hello modul Správa verzí omezení hello **package.json** souboru. Při hello **package.json** souboru obsahuje pouze verze pro moduly nejvyšší úrovně hello hello **npm shrinkwrap.json** soubor obsahuje hello požadavky verze pro řetězec závislostí úplné modulu hello.

Když je aplikace připravená pro vydání, můžete uzamknout požadavky verze a vytvořit **npm shrinkwrap.json** souboru pomocí hello **npm shrinkwrap** příkaz. Tento příkaz bude používat na hello aktuálně nainstalovaná verze hello **uzlu\_moduly** složku a zaznamenejte tyto verze toohello **npm shrinkwrap.json** souboru. Po hello aplikace nasazené toohello hostování prostředí, hello **npm nainstalujte** příkaz je použité tooparse hello **npm shrinkwrap.json** souboru a nainstalujte všechny uvedené závislosti hello. Další informace najdete v tématu [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> Pokud se nasazuje tooAzure služby App Service vaše <b>npm shrinkwrap.json</b> soubor odkazuje na nativní modul, může dojít k chybě podobné toohello následující ukázka při publikování aplikace hello pomocí Git:
> 
> npm ERR! module-name@0.6.0Nainstalujte: 'uzlu gyp konfigurace buildu.
> 
> npm ERR! ' cmd "/ c" "uzlu gyp konfigurace sestavení" ' se nezdařilo s 1
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když znáte jak toouse modulů Node.js s Azure, zjistěte, jak příliš[určení verze Node.js hello], [sestavení a nasazení webové aplikace Node.js](app-service-web/app-service-web-get-started-nodejs.md), a [jak toouse hello příkazového řádku Azure Rozhraní pro Mac a Linux].

Další informace najdete v tématu hello [středisku pro vývojáře Node.js](/nodejs/azure/).

[určení verze Node.js hello]: nodejs-specify-node-version-azure-apps.md
[jak toouse hello příkazového řádku Azure Rozhraní pro Mac a Linux]:cli-install-nodejs.md
[skriptů nasazení vlastní web s Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
