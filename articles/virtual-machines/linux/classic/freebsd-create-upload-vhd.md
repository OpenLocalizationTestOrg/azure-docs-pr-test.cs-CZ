---
title: "Obrázek aaaCreate a nahrání virtuálního počítače FreeBSD | Microsoft Docs"
description: "Další informace toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello FreeBSD operačního systému toocreate virtuální počítač Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a>Vytvoření a nahrání virtuálního pevného disku FreeBSD tooAzure
Tento článek ukazuje, jak toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello FreeBSD operačního systému. Po odeslání, můžete jej jako vlastní image toocreate virtuální počítač (VM) v Azure.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o nahrávání virtuální pevný disk pomocí modelu Resource Manager hello najdete v tématu [zde](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte hello následující položky:

* **Předplatné Azure**– Pokud nemáte účet, můžete vytvořit jeden si během několika minut. Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Jinak, zjistěte, jak příliš[vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).  
* **Nástroje Azure PowerShell**– modul Azure PowerShell hello musí být nainstalován a nakonfigurován toouse vašeho předplatného Azure. modul hello toodownload, najdete v části [Azure stáhne](https://azure.microsoft.com/downloads/). Kurz, který popisuje, jak tooinstall a nakonfigurovat modul hello Zde jsou k dispozici. Použití hello [Azure stáhne](https://azure.microsoft.com/downloads/) rutiny tooupload hello virtuálního pevného disku.
* **FreeBSD operační systém nainstalovaný v souboru VHD**– na podporovaný operační systém FreeBSD musí být nainstalován tooa virtuální pevný disk. Několik nástrojů existovat toocreate soubory VHD. Můžete například použít řešení virtualizace jako soubor VHD hello toocreate technologie Hyper-V a nainstalujte hello operační systém. Pokyny o tom, jak tooinstall a použití technologie Hyper-V, najdete v části [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Hello novější formát VHDX není podporovaný v Azure. Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny [convert-VHD prostředí](https://technet.microsoft.com/library/hh848454.aspx). Kromě toho je [kurz na webu MSDN o toouse FreeBSD s technologií Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Tato úloha obsahuje hello pět kroků:

## <a name="step-1-prepare-hello-image-for-upload"></a>Krok 1: Příprava hello image pro odesílání
Na virtuálním počítači hello nainstalovanou hello FreeBSD operačního systému proveďte následující postupy hello:

1. Povolte protokol DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. Povolte SSH.

    Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění. Ve výchozím nastavení je povoleno po instalaci z disku FreeBSD. 
3. Nastavení konzoly sériového portu.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Nainstalujte sudo.

    Hello kořenového účtu je zakázána v Azure. To znamená, že potřebujete tooutilize sudo z Neprivilegovaný uživatelský toorun příkazy se zvýšenými oprávněními.

        # pkg install sudo
   
5. Požadavky pro agenta Azure.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Nainstalujte agenta Azure.

    Hello nejnovější verzi agenta Azure hello vždy naleznete na [githubu](https://github.com/Azure/WALinuxAgent/releases). Hello verze 2.0.10 + oficiálně podporuje FreeBSD 10 & 10.1 a verze hello 2.1.4 + (včetně 2.2.x) oficiálně podporuje FreeBSD 10.2 a pozdějších verzích.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Pro 2.0 použijeme 2.0.16 jako příklad:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Pro 2.1 použijeme 2.1.4 jako příklad:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Po instalaci agenta Azure, je vhodné tooverify, který používá:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Zrušení zřízení hello systému.

    Deprovision hello systému tooclean ho a zkontrolujte ji vhodný pro reprovisioning. Hello následující příkaz dojde také k odstranění hello poslední zřízené uživatelský účet a hello související data:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nyní můžete vypnout virtuální počítač.

## <a name="step-2-create-a-storage-account-in-azure"></a>Krok 2: Vytvoření účtu úložiště v Azure
Potřebujete účet úložiště v Azure tooupload soubor VHD, může být použité toocreate virtuálního počítače. Můžete použít hello Azure classic portálu toocreate účet úložiště.

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Na panelu příkazů hello, vyberte **nový**.
3. Vyberte **datové služby** > **úložiště** > **rychle vytvořit**.

    ![Rychlé vytvoření účtu úložiště](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Vyplňte pole hello následujícím způsobem:

   * V hello **URL** pole, zadejte toouse název subdomény hello adresa URL účtu úložiště. Hello záznam může obsahovat ze 3 – 24 čísla a malá písmena. Tento název se změní na název hostitele hello v rámci hello adresu URL, která je použité tooaddress Azure Blob storage, Azure Queue storage nebo Azure Table storage prostředky pro předplatné hello.
   * V hello **umístění/skupině vztahů** rozevírací nabídky vyberte hello **umístění nebo skupina vztahů** pro účet úložiště hello. Skupiny vztahů umožňuje uvést vaše cloudové služby a úložiště do hello stejném datovém centru.
   * V hello **replikace** pole, rozhodnout, zda toouse **geograficky redundantní** replikace pro účet úložiště hello. Geografická replikace je zapnutá ve výchozím nastavení. Tato možnost replikuje vaše data tooa sekundárního umístění, v žádné tooyou náklady, tak, aby úložiště převezme toothat umístění, pokud hlavní selhání nastane v primárním umístění hello. Hello sekundárního umístění je přiřazen automaticky a nedá se změnit. Pokud potřebujete větší kontrolu nad hello umístění cloudové úložiště z důvodu toolegal požadavky nebo zásad organizace, můžete vypnout geografická replikace. Upozorňujeme však, že pokud později zapnout geografická replikace, vám bude účtována jednorázové přenos dat poplatek tooreplicate vaší existující data toohello sekundárního umístění. Služby úložiště bez geografická replikace se nabízí se slevou. Další informace o správě geografická replikace účtů úložiště najdete tady: [replikace Azure Storage](../../../storage/common/storage-redundancy.md).

     ![Zadejte podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Vyberte **vytvořit účet úložiště**. Hello účet se teď zobrazí v části **úložiště**.

    ![Účet úložiště byl úspěšně vytvořen](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Dále vytvořte kontejner pro vaše soubory nahrávat VHD. Vyberte název účtu úložiště hello a pak vyberte **kontejnery**.

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Vyberte **vytvořit kontejner**.

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. V hello **název** pole, zadejte název vašeho kontejneru. Potom v hello **přístup** rozevírací nabídky, vyberte typ zásady přístupu, které chcete.

    ![Název kontejneru](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Ve výchozím kontejneru hello je privátní a můžete přistupovat pouze hello vlastníka účtu. tooallow veřejný přístup pro čtení toohello objektů BLOB v kontejneru hello, ale toohello vlastnosti kontejneru a metadata, použijte hello **veřejného objektu Blob** možnost. tooallow úplné veřejný přístup pro čtení hello kontejneru a objekty BLOB, použijte hello **veřejném kontejneru** možnost.
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a>Krok 3: Příprava tooAzure připojení hello
Můžete nahrát soubor VHD, musíte tooestablish zabezpečené připojení mezi vaším počítačem a vašeho předplatného Azure. Můžete použít metoda hello Azure Active Directory (Azure AD), nebo hello certifikát metoda toodo ho.

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a>Použít hello Azure AD metoda tooupload soubor VHD
1. Otevřete hello konzoly Azure PowerShell.
2. Zadejte hello následující příkaz:  
    `Add-AzureAccount`

    Tento příkaz otevře okno přihlášení, kde se můžete přihlásit pomocí svého pracovního nebo školního účtu.

    ![Okno prostředí PowerShell](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure ověřuje a uloží hello přihlašovacích údajů. Potom zavře okno hello.

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a>Použít hello certifikát metoda tooupload soubor VHD
1. Otevřete hello konzoly Azure PowerShell.
2. Typ: `Get-AzurePublishSettingsFile`.
3. Okno prohlížeče se otevře a vyzve jste toodownload hello soubor .publishsettings. Tento soubor obsahuje informace a certifikát pro vaše předplatné Azure.

    ![Stránka pro stažení prohlížeče](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Uložte soubor .publishsettings hello.
5. Typ: `Import-AzurePublishSettingsFile <PathToFile>`, kde `<PathToFile>` je soubor .publishsettings toohello hello úplnou cestu.

   Další informace najdete v tématu [Začínáme s Azure rutiny](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Další informace o instalaci a konfiguraci prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="step-4-upload-hello-vhd-file"></a>Krok 4: Nahrání souboru VHD hello
Při nahrávání souboru VHD hello, můžete je umístit kamkoli do úložiště objektů Blob. Tady jsou některé podmínky, které budete používat při nahrávání souboru hello:

* **BlobStorageURL** hello adresa URL pro účet úložiště hello, kterou jste vytvořili v kroku 2.
* **YourImagesFolder** je hello kontejneru v úložiště objektů Blob místo toostore obrázků.
* **VHDName** je hello štítek, který se zobrazí v Azure classic portálu tooidentify hello hello virtuální pevný disk.
* **PathToVHDFile** je hello úplná cesta a název souboru VHD hello.

Hello okno Azure PowerShell, které jste použili v předchozí krok text hello zadejte:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a>Krok 5: Vytvoření virtuálního počítače pomocí souboru nahrávat VHD hello
Po odeslání souboru VHD hello, můžete ji přidat jako seznam vlastních bitových kopií, které jsou spojeny s předplatným a vytvoření virtuálního počítače s touto bitovou kopií vlastní image toohello.

1. Hello okno Azure PowerShell, které jste použili v předchozí krok text hello zadejte:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > Použijte jako typ hello operačního systému Linux. aktuální verzi prostředí Azure PowerShell Hello přijímá pouze "Linux" nebo "Systém Windows" jako parametr.
   >
   >
2. Po dokončení předchozích kroků hello je uveden hello novou bitovou kopii, když zvolíte hello **bitové kopie** karty v hello portál Azure classic.  

    ![Vyberte bitovou kopii](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Vytvoření virtuálního počítače z Galerie hello. Tuto novou bitovou kopii je nyní k dispozici v části **Moje image**.
4. Výběr nové image hello. V dalším kroku projít hello výzvy tooset název hostitele, heslo, klíč SSH a tak dále.

    ![Vlastní image](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Po dokončení zřizování hello uvidíte FreeBSD virtuální počítač běží v Azure.

    ![Obrázek FreeBSD v azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
