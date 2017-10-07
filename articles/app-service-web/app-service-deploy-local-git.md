---
title: "aaaLocal nasazení Git tooAzure služby App Service"
description: "Zjistěte, jak tooenable místní Git nasazení tooAzure služby App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a>Místní nasazení Git tooAzure služby App Service
Tento kurz ukazuje, jak toodeploy aplikace příliš[Azure App Service] z úložiště Git v místním počítači. App Service podporuje tento přístup s hello **místní Git** možnost nasazení v hello [portálu Azure].  
Mnoho příkazů Git hello popsané v tomto článku se provádí automaticky při vytváření aplikace služby App Service pomocí hello [rozhraní příkazového řádku Azure] popsané [zde](app-service-web-get-started.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu potřebujete:

* Git. Si můžete stáhnout hello instalace binární [zde](http://www.git-scm.com/downloads).  
* Základní znalosti o Git.
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.  
> 
> 

## <a name="Step1"></a>Krok 1: Vytvoření místní úložiště
Proveďte následující úlohy toocreate nového úložiště Git hello.

1. Spusťte nástroj příkazového řádku, jako například **Git Bash** (Windows) nebo **Bash** (Unix prostředí). V systémech OS X můžete přistupovat prostřednictvím hello hello příkazového řádku **Terminálové** aplikace.
2. Přejděte toohello adresáře, kde bude umístěn obsahu toodeploy hello.
3. Použijte následující příkaz tooinitialize nového úložiště Git hello:

```bash  
git init
```

## <a name="Step2"></a>Krok 2: Potvrzení obsahu
App Service podporuje aplikace vytvořené v různých programovacích jazyků. 

1. Pokud úložiště už obsahuje tento bod obsahu přeskočit a přesunout toopoint 2 níže. Pokud úložiště už neobsahuje obsah jednoduše naplnění soubor statické .html následujícím způsobem: 
   
   * Pomocí textového editoru, vytvořte nový soubor s názvem **index.html** v kořenovém adresáři úložiště Git hello hello
   * Přidejte následující text jako hello obsah pro hello index.html souboru a uložte ho hello: *Hello Git!*
2. Z příkazového řádku hello ověřte, že jste v kořenovém adresáři hello úložiště Git. Pak použijte následující příkaz tooadd soubory tooyour úložiště hello:

```bash  
git add -A
```
3. V dalším kroku potvrdit hello změny toohello úložiště pomocí hello následující příkaz:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>Krok 3: Povolení hello úložiště aplikace služby App Service
Proveďte následující kroky tooenable úložiště Git pro vaši aplikaci služby App Service hello.

1. Přihlaste se toohello [portálu Azure].
2. V okně aplikace App Service, klikněte na tlačítko **Nastavení > zdroj nasazení**. Klikněte na tlačítko **vybrat zdroj**, pak klikněte na tlačítko **místní úložiště Git**a potom klikněte na **OK**.  
   
    ![Místní úložiště Git](./media/app-service-deploy-local-git/local_git_selection.png)
3. Pokud je vaše první čas nastavení úložiště v Azure, musíte pro něj toocreate přihlašovací údaje. Použijete je toolog do hello úložiště Azure a změny z místního úložiště Git. V okně vaší aplikace, klikněte na tlačítko **Nastavení > přihlašovací údaje nasazení**, nakonfigurujte nasazení uživatelské jméno a heslo. Když jste hotovi, klikněte na tlačítko **Uložit**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Nasazení projektu
Pomocí následujících kroků toopublish hello tooApp vaší aplikace pomocí Git místní služby.

1. V okně vaší aplikace v hello portálu Azure, klikněte na tlačítko **Nastavení > vlastnosti** pro hello **adresy URL pro Git**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Adresy URL pro Git** je hello vzdáleném referenčním toodeploy toofrom místní úložiště. Tato adresa URL budete používat v hello následující kroky.
2. Pomocí příkazového řádku hello, ověřte, že jste v kořenovém hello místní úložiště Git.
3. Použití `git remote` tooadd hello vzdáleném referenčním uvedené v **adresy URL pro Git** z kroku 1. Váš příkaz bude vypadat podobně jako toohello následující:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > Hello **vzdáleného** příkaz přidá tooa vzdáleného úložiště s názvem odkaz. V tomto příkladu se vytvoří odkaz úložiště vaší webové aplikace s názvem "azure".
   > 
   > 
4. Push vaší obsahu tooApp služby pomocí nové hello **azure** vzdálené jste právě vytvořili.

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. Vraťte se zpátky tooyour aplikace v hello portálu Azure. Položka protokolu z vaší poslední nabízené má být zobrazena v hello **nasazení** okno. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Klikněte na tlačítko hello **Procházet** tlačítko hello horní části aplikace hello okno tooverify hello obsahu byla nasazena. 

## <a name="Step5"></a>Řešení potíží
Hello následují chyby nebo problémy, které jsou běžně došlo při použití Git toopublish tooan aplikaci služby App Service v Azure:

- - -
**Příznaky**: nelze tooaccess [siteURL]: nepodařilo tooconnect příliš [scmAddress]

**Příčina**: k této chybě může dojít, pokud aplikace hello není spuštěná.

**Řešení**: spuštění aplikace hello v hello portálu Azure. Nasazení Git nebude fungovat, pokud běží aplikace hello. 

- - -
**Příznaky**: nelze přeložit název hostitele, hostitele.

**Příčina**: k této chybě může dojít, pokud informace o adrese hello zadali při vytváření hello "azure" vzdálené nebyla správná.

**Řešení**: použití hello `git remote -v` příkaz toolist dálkové všechny ovladače, společně s hello přidružené adresy URL. Ověřte, že adresa URL hello hello "azure" vzdálené správné. V případě potřeby odeberte a znovu tento vzdálený pomocí hello správnou adresu URL.

- - -
**Příznaky**: žádné odolný systém souborů v běžné a žádná zadaný; žádným způsobem. Možná musíte zadat větve, jako je například "hlavní".

**Příčina**: k této chybě může dojít, pokud jste nezadávejte větev při provádění operace git push a mít není sada hello push.default hodnota používaná metodou Git.

**Řešení**: hello nabízené znovu operaci provést, zadání hello hlavní větve. Například:

```bash  
git push azure master
```
- - -
**Příznaky**: src refspec [branchname] neodpovídá žádnému.

**Příčina**: této chybě může dojít, pokud se pokusíte větve tooa toopush než hlavní server na hello "azure" vzdálené.

**Řešení**: hello nabízené znovu operaci provést, zadání hello hlavní větve. Například:

```bash  
git push azure master
```
- - -
**Příznaky**: RPC se nezdařilo; způsobit = 22, kód HTTP = 502.

**Příčina**: této chybě může dojít, pokud se pokusíte toopush úložiště git velké přes protokol HTTPS.

**Řešení**: Změna konfigurace git hello na hello místního počítače toomake hello postBuffer větší

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Příznaky**: Chyba: změny potvrzeny tooremote úložiště ale vaší webové aplikace nejsou aktualizovány.

**Příčina**: této chybě může dojít, pokud nasazujete aplikaci Node.js obsahující soubor package.json, který určuje další požadované moduly.

**Řešení**: další zprávy obsahující 'npm ERR!. musí být zaznamenané toothis předchozí chyby a může poskytnout další kontext na selhání hello. Následující Hello se ví, že příčiny této chyby a hello odpovídající 'npm ERR!. zpráva:

* **Soubor package.json chybná**: npm ERR! Nepodařilo se přečíst závislosti.
* **Nativní modul, který nemá binární distribuce pro systém Windows**:
  
  * npm ERR! \`cmd "/ c" "uzlu gyp opětovné sestavení"\` se nezdařilo s 1
    
      NEBO
  * npm ERR! [modulename@version] předinstalovat: \`zkontrolujte || gmake\`

## <a name="additional-resources"></a>Další zdroje
* [Dokumentace pro Git](http://git-scm.com/documentation)
* [Dokumentace Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Nepřetržité nasazení tooAzure služby App Service](app-service-continuous-deployment.md)
* [Jak toouse prostředí PowerShell pro Azure](/powershell/azure/overview)
* [Jak toouse hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[portálu Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[rozhraní příkazového řádku Azure]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
