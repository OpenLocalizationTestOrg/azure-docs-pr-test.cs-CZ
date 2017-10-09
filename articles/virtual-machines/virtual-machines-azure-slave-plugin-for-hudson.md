---
title: "aaaHow toouse hello Azure podřízený modul plug-in s průběžnou integraci Hudsonem | Microsoft Docs"
description: "Popisuje, jak toouse hello Azure podřízený s Hudsonem průběžnou integraci modulu plug-in."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a>Jak toouse hello Azure podřízený s Hudsonem průběžnou integraci modulu plug-in
Hello Azure podřízený modulu plug-in pro Hudsonem umožňuje tooprovision podřízené uzly v Azure při spuštění distribuované sestavení.

## <a name="install-hello-azure-slave-plug-in"></a>Instalace modulu plug-in Azure podřízený hello
1. V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.
2. V hello **spravovat Hudsonem** klikněte na **Správa modulů plug-in**.
3. Klikněte na tlačítko hello **dostupné** kartě.
4. Klikněte na tlačítko **vyhledávání** a typ **Azure** toolimit hello seznamu toorelevant zásuvné moduly.
   
    Pokud se přihlásíte tooscroll prostřednictvím hello seznamu dostupných modulů plug-in, zjistí hello Azure podřízený modulu plug-in pod hello **správu clusteru a distribuované sestavení** část v hello **ostatní** kartě.
5. Zaškrtněte políčko hello pro **modul plug-in Azure podřízený**.
6. Klikněte na **Nainstalovat**.
7. Restartujte Hudsonem.

Nyní je nainstalován tento modul plug-in hello, bude další kroky hello tooconfigure hello modul plug-in s profil předplatného Azure a toocreate šablonu, která se použije při vytváření hello virtuálních počítačů pro hello podřízený uzel.

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a>Hello Azure podřízený modul plug-in nakonfigurovat svůj profil předplatného
Profil předplatné také odkazované tooas nastavení publikování, je soubor XML, který obsahuje zabezpečené přihlašovací údaje a některé další informace, které budete potřebovat toowork s Azure ve vašem vývojovém prostředí. tooconfigure hello Azure podřízený modul plug-in, budete potřebovat:

* Vaše id odběru
* Certifikát pro správu pro vaše předplatné

Ty lze najít ve vaší [odběru profil]. Dole je příklad profilu předplatného.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Jakmile je váš profil předplatného, postupujte podle těchto kroků tooconfigure hello Azure podřízený modulu plug-in.

1. V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.
2. Klikněte na tlačítko **konfiguraci systému**.
3. Projděte dolů hello toofind stránku hello **cloudu** části.
4. Klikněte na tlačítko **přidat nové cloudové > Microsoft Azure**.
   
    ![Přidat nové cloudu][add new cloud]
   
    Pole text hello, kde je nutné tooenter zobrazí podrobnosti o vašem předplatném.
   
    ![Konfigurace profilu][configure profile]
5. Zkopírovat hello předplatné id a správy certifikátu z profilu předplatného a vložte je do příslušných polí hello.
   
    Při kopírování id a správy certifikátu předplatného hello **nepodporují** zahrnují hello uvozovky, které uzavřete hello hodnoty.
6. Klikněte na **ověřte konfiguraci**.
7. Po konfiguraci hello je ověření bylo úspěšné, klikněte na tlačítko **Uložit**.

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a>Nastavení šablony virtuálního počítače pro hello Azure podřízený modulu plug-in
Šablonu virtuálního počítače definuje parametry hello hello modul plug-in použije toocreate podřízený uzel v Azure. V následující kroky hello jsme budete vytvoření šablony pro virtuálního počítače s Ubuntu.

1. V hello Hudsonem řídicí panel, klikněte na **spravovat Hudsonem**.
2. Klikněte na **konfiguraci systému**.
3. Projděte dolů hello toofind stránku hello **cloudu** části.
4. V rámci hello **cloudu** vyhledejte **přidat šablonu virtuálního počítače Azure** a klikněte na tlačítko hello **přidat** tlačítko.
   
    ![Přidat šablonu virtuálního počítače.][add vm template]
5. Zadejte název cloudové služby v hello **název** pole. Pokud zadáte název hello odkazuje tooan stávající cloudovou službu, hello virtuálního počítače se zřídí v této službě. Jinak Azure vytvoří novou.
6. V hello **popis** pole, zadejte text, který popisuje hello šablonu, kterou vytváříte. Tyto informace je pouze pro účely písemné a nepoužívá se v zřizování virtuálního počítače.
7. V hello **popisky** zadejte **linux**. Tento popisek je použité tooidentify hello šablonu, kterou vytváříte a následně použít tooreference hello šablonu při vytváření úlohy Hudsonem.
8. Vyberte oblast, kde bude vytvořen hello virtuálních počítačů.
9. Vyberte odpovídající velikost virtuálního počítače hello.
10. Zadejte účet úložiště, kde bude vytvořen hello virtuálních počítačů. Ujistěte se, že je v hello stejné oblasti jako hello cloudové služby, které budete používat. Pokud chcete vytvořit nové úložiště toobe, můžete toto pole zůstat prázdné.
11. Doba uchování určuje hello počet minut, než Hudsonem odstraní nečinnosti podřízený. Nechte na hello výchozí hodnotu 60.
12. V **využití**, vyberte hello vhodné podmínky, když se použije tento podřízený uzel. Nyní, vyberte **využívají tento uzel co nejvíce**.
    
     V tomto okamžiku by formulář vypadat poněkud podobný toothis:
    
     ![Konfigurace šablony][template config]
13. V **řady bitovou kopii nebo Id** máte toospecify jaké bitové kopie systému bude nainstalována na váš počítač. Můžete vybrat ze seznamu rodin bitové kopie, nebo zadejte vlastní image.
    
     Pokud chcete tooselect ze seznamu rodiny bitové kopie, zadejte hello první znak (malá a velká písmena) název rodiny hello bitové kopie. Například zadáním **U** zobrazíte seznam rodiny Ubuntu Server. Jakmile vyberete ze seznamu hello volaných používat nejnovější verzi této bitové kopie systému z této rodiny hello při zřízení virtuálního počítače.
    
     ![Rodiny seznamu operačního systému][OS family list]
    
     Pokud máte vlastní image, které chcete toouse místo toho, zadejte název této vlastní image hello. Názvy vlastních obrázků se nezobrazí v seznamu, abyste získali, že tooensure, který hello název zadán správně.    
    
     V tomto kurzu zadejte **U** toobring seznam Image Ubuntu a vyberte **Ubuntu Server 14.04 LTS**.
14. Pro **spusťte metoda**, vyberte **SSH**.
15. Zkopírujte níže hello skriptu a vložte hello **Init skriptu** pole.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     Hello **Init skriptu** bude proveden po hello vytvoří virtuální počítač. V tomto příkladu hello skript nainstaluje ant, Java a git.
16. V hello **uživatelské jméno** a **heslo** pole, zadejte upřednostňované hodnoty pro účet správce hello, která bude vytvořena na vašem virtuálním počítači.
17. Klikněte na **ověřte šablony** toocheck Pokud hello parametry, které jste zadali platné.
18. Klikněte na **Uložit**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Vytvořit úlohu Hudsonem, který běží na uzlu podřízený v Azure
V této části budete vytváření Hudsonem úlohu, která se spustí na podřízený uzel v Azure.

1. V hello Hudsonem řídicí panel, klikněte na **nová úloha**.
2. Zadejte název pro hello úlohu, kterou vytváříte.
3. Hello typ úlohy, vyberte **sestavení úloha softwaru bez stylu**.
4. Klikněte na **OK**.
5. Na stránce konfigurace hello úlohy, vyberte **omezit, kde můžete spustit tento projekt**.
6. Vyberte **uzlu a popisek nabídky** a vyberte **linux** (jsme zadali tento popisek, při vytváření šablony virtuálního počítače hello v předchozí části hello).
7. V hello **sestavení** klikněte na tlačítko **přidat krok sestavení** a vyberte **spustit prostředí**.
8. Upravit hello následující skript, nahraďte **{název účtu github}**, **{název projektu}**, a **{adresáři projektu}** s příslušným hodnoty a vložit hello Upravit skript v hello textová oblast, která se zobrazí.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. Klikněte na **Uložit**.
10. V hello Hudsonem řídicí panel, najděte hello úlohu, kterou jste právě vytvořili a klikněte na hello **naplánovat sestavení** ikonu.

Hudsonem se pak vytvořit pomocí šablony hello vytvořili v předchozí části hello podřízený uzel a spuštění hello skriptu, který jste zadali v kroku hello sestavení pro tuto úlohu.

## <a name="next-steps"></a>Další kroky
Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

<!-- URL List -->

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[odběru profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

