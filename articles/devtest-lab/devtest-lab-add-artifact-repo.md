---
title: "aaaAdd úložiště Git tooa testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Přidejte úložiště GitHub nebo Visual Studio Team Services Git pro váš vlastní artefakty zdroj v Azure DevTest Labs"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: e590559ffb2d497e39823e35c3f66974f42f13c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-git-repository-toostore-custom-artifacts-and-azure-resource-manager-templates"></a>Přidání vlastních artefaktů Git úložiště toostore a šablon Azure Resource Manageru

Pokud chcete příliš[vytvoření vlastních artefaktů](devtest-lab-artifact-author.md) pro hello virtuálních počítačů ve svém testovacím prostředí, nebo [toocreate šablony Azure Resource Manager vlastní testovacím prostředí použít](devtest-lab-create-environment-from-arm.md), musíte taky přidat privátní tooinclude úložiště Git artefakty Hello nebo šablon Azure Resource Manageru, které vytvoří tým. může být hostitelem úložiště Hello [Githubu](https://github.com) nebo na [Visual Studio Team Services (VSTS)](https://visualstudio.com).

Uvádíme [úložiště Github artefaktů](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) , kterou můžete nasadit jako-je nebo upravit pro vaše prostředí. Když se upravit nebo vytvořit artefakt, nelze uložit je do veřejného úložiště hello – musíte vytvořit vlastní privátní úložiště. 

Při vytváření virtuálního počítače můžete uložit hello šablony Azure Resource Manageru, přizpůsobit, pokud chcete a pak jeho pomocí novější tooeasily vytvořit další virtuální počítače. Vlastní privátní úložiště toostore musíte vytvořit vlastní šablony Azure Resource Manager.  

* jak zjistit, toocreate úložišti GitHub, toolearn [Githubu Bootcamp](https://help.github.com/categories/bootcamp/).
* jak zjistit, toocreate projektu Team Services s podpůrným úložištěm Git, toolearn [připojit tooVisual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

Hello následující snímek obrazovky ukazuje příklad jak může vypadat úložiště obsahující artefakty v Githubu:  
![Úložiště artefaktů ukázka GitHub](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-hello-repository-information-and-credentials"></a>Získat informace o úložišti hello a přihlašovací údaje
tooadd testovacího prostředí tooyour úložiště, musíte nejprve získat určité informace z vašeho úložiště. Následující části Hello provede načtení těchto informací pro úložiště, které jsou hostované na Githubu a Visual Studio Team Services.

### <a name="get-hello-github-repository-clone-url-and-personal-access-token"></a>Získat hello adresa URL klonování úložiště GitHub a osobní přístupový token
Adresa URL klonování tooget hello Githubu úložiště a osobní přístupový token, postupujte takto:

1. Procházet toohello domovské stránce úložiště GitHub hello, který obsahuje hello artefaktů nebo definice šablony Azure Resource Manager.
2. Vyberte **klonovat nebo stáhnout**.
3. Vyberte hello tlačítko toocopy hello **url naklonovaného HTTPS** toohello schránky a uložte hello adresu URL pro pozdější použití.
4. Vyberte profil hello bitové kopie v pravém horním rohu hello Githubu a vyberte **nastavení**.
5. V hello **osobní nastavení** nabídce vlevo, hello vyberte **osobní přístupové tokeny**.
6. Vyberte **vygenerovat nový token**.
7. Na hello **nový osobní přístupový token** stránky, zadejte **Token popis**, přijměte výchozí položky hello v hello **vyberte obory**a potom zvolte **generování Token**.
8. Hello vygenerovat token uložte, protože ji budete potřebovat později.
9. Nyní můžete zavřít Githubu.   
10. Pokračovat toohello [připojení úložiště toohello testovacím](#connect-your-lab-to-the-repository) části.

### <a name="get-hello-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>Získat adresu URL klonu hello Visual Studio Team Services úložiště a osobní přístupový token
Adresa URL úložiště klonování tooget hello Visual Studio Team Services a osobní přístupový token, postupujte takto:

1. Hello otevřete domovskou stránku vaší kolekce team (například `https://contoso-web-team.visualstudio.com`) a potom vyberte projektu.
2. Na domovské stránce hello projekt, vyberte **kód**.
3. tooview hello adresa URL klonování, na projektu hello **kód** vyberte **klon**.
4. Adresa URL hello uložte, protože ho potřebovat později v tomto kurzu.
5. Vyberte toocreate osobní přístup tokenu, **Můj profil** hello uživatele účtu rozevírací nabídce.
6. Na stránce informace o profilu hello vyberte **zabezpečení**.
7. Na hello **zabezpečení** vyberte **přidat**.
8. V hello **vytvoření osobního přístupového tokenu** stránky:

   * Zadejte **popis** pro hello token.
   * Vyberte **180 dnů** z hello **vyprší v** seznamu.
   * Zvolte **všechny dostupné účty** z hello **účty** seznamu.
   * Zvolte hello **všechny obory** možnost.
   * Zvolte **vytvořit Token**.
9. Po dokončení se zobrazí nový token hello v hello **osobní přístupové tokeny** seznamu. Vyberte **kopie tokenu**a potom uložte hello hodnota tokenu pro pozdější použití.
10. Pokračovat toohello [připojení úložiště toohello testovacím](#connect-your-lab-to-the-repository) části.

## <a name="connect-your-lab-toohello-repository"></a>Připojení úložiště toohello testovacího prostředí
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello.   
4. Na levém panelu hello, vyberte **konfiguraci a zásady**.
5. V testovacím hello **konfiguraci a zásady** oblasti, vyberte **úložiště**.
6. Na hello **úložiště** oblasti, vyberte **+ přidat**.

    ![Úložiště tlačítko Přidat](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
7. Na hello druhý **úložiště** zadejte hello následující informace:

   * **Název** – zadejte název úložiště hello.
   * **Adresa Url klonu Git** -zadejte hello Git HTTPS klon URL, který jste zkopírovali dříve z webu GitHub nebo Visual Studio Team Services.
   * **Větev** -zadejte hello větve tooget definic.
   * **Osobní tokenu přístupu** -zadejte hello osobní přístupový token jste dříve získali z Githubu nebo Visual Studio Team Services.
   * **Cesty ke složce zadat** -zadejte alespoň jednu složku cesta relativní toohello klon URL, která obsahuje vaše artefaktů nebo definice šablony Azure Resource Manager. Při zadávání podadresáři, ujistěte se, že tooinclude hello lomítkem hello ve složce v cestě.

     ![Oblast úložiště](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
8. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
Jakmile vytvoříte privátní úložiště Git, můžete provést jednu nebo obě následující hello, v závislosti na vašich potřeb:
* Úložiště vaše [vlastních artefaktů](devtest-lab-artifact-author.md), které můžete použít novější toocreate nové virtuální počítače.
* [Vytvoření prostředí více virtuálních počítačů a PaaS prostředky pomocí šablony Azure Resource Manager](devtest-lab-create-environment-from-arm.md) a potom uložte hello šablony ve vaší privátní úložišti.

Když vytvoříte virtuální počítač, můžete ověřit, šablony nebo hello artefaktů přidají tooyour úložiště Git. Jsou k dispozici okamžitě v seznamu hello artefakty nebo šablony, hello název vaší privátní úložiště uvedené v hello sloupec, který určuje hello zdroj. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="related-blog-posts"></a>Příspěvky blogu související
* [Jak tootroubleshoot selhání artefakty v Azure DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Připojení k tooexisting virtuálního počítače domény AD pomocí šablony správce prostředků v Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)
