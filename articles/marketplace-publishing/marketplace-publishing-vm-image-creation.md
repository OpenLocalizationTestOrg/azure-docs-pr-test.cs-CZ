---
title: "aaaCreating bitovou kopii virtuálního počítače pro hello Azure Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate virtuálního počítače bitové kopie pro hello Azure Marketplace pro ostatní toopurchase."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 65e1c0530bb050fb379a52544e36c55faacd84df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-virtual-machine-image-for-hello-azure-marketplace"></a>Průvodce toocreate bitovou kopii virtuálního počítače pro hello Azure Marketplace
Tento článek **kroku 2**, vás provede procesem přípravy hello virtuální pevné disky (VHD), že budete nasazovat toohello Azure Marketplace. Virtuální pevné disky jsou hello základ pro vaše SKU. Hello proces se liší v závislosti na tom, jestli tím SKU systémem Linux nebo systému Windows. Tento článek se týká obou scénářů. Tento postup lze provést paralelně s [vytváření účtů a registrace][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Definování nabídky a SKU
V této části se dozvíte toodefine hello nabídky a jejich přidružené SKU.

Nabídka je "nadřazená" tooall z jeho SKU. Nabídek může být víc. Jak se rozhodnete toostructure vaší nabídky je tooyou. Pokud je nabídka vložena toostaging, je vložena společně se všemi jeho SKU. Pečlivě zvažte vaše identifikátory SKU, protože se budou viditelné v adrese URL hello:

* Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}
* Portál Azure preview: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}  

SKU je hello komerční název image virtuálního počítače. Image virtuálního počítače obsahuje disk jeden operační systém a nula nebo více datových disků. Je v podstatě profil hello dokončení úložiště pro virtuální počítač. Jeden virtuální pevný disk je potřeba na disk. Data i prázdné disky se vyžaduje virtuálního pevného disku toobe, vytvořit.

Bez ohledu na to, jaký používáte operační systém přidejte jenom hello minimální počet datových disků vyžaduje hello SKU. Zákazníci disky, které jsou součástí bitové kopie v době nasazení hello nelze odebrat, ale můžete vždy přidat disky během nebo po nasazení Pokud je potřebují.

> [!IMPORTANT]
> **Neměňte počet disků v nové verzi bitové kopie.** Pokud je nutné překonfigurovat datové disky v bitové kopii hello, definujte nové SKU. Publikování nové verze bitové kopie s počty jiný disk, bude mít potenciálně hello nejnovější nové nasazení na základě hello nové verze bitové kopie v případech automatické škálování, automatické nasazení řešení pomocí šablony ARM a jiné scénáře.
>
>

### <a name="11-add-an-offer"></a>1.1 přidání nabídky
1. Přihlaste se toohello [publikování portál] [ link-pubportal] pomocí účtu prodejce.
2. Vyberte hello **virtuální počítače** kartě hello publikování portálu. V poli s výzvami položkou hello zadejte název nabídky. Název nabídky Hello je obvykle název hello hello produktu nebo služby, abyste naplánovali toosell v hello Azure Marketplace.
3. Vyberte **Vytvořit**.

### <a name="12-define-a-sku"></a>1.2 definice SKU
Po přidání nabídku, potřebujete toodefine a identifikaci vaší SKU. Můžete mít více nabídkami a každý nabídka může mít více SKU v něm. Pokud je nabídka vložena toostaging, je vložena společně se všemi jeho SKU.

1. **Přidáte SKU.** Hello SKU vyžaduje identifikátor, který se používá v adrese URL hello. identifikátor Hello musí být jedinečný v rámci vaší profil publikování, ale neexistuje žádné riziko identifikátor kolizí s jinými vydavateli.

   > [!NOTE]
   > Nabídka Hello a identifikátory SKU se zobrazí v adrese URL nabídka hello v hello Marketplace.
   >
   >
2. **Přidejte souhrnný popis pro vaše SKU.** Souhrn popisy jsou viditelné toocustomers, takže si vytvořit snadno čitelné. Tyto informace nemusí toobe uzamčení dokud fáze "Push tooStaging" hello. Do té doby se volné tooedit ho.
3. Pokud používáte SKU systému Windows, postupujte podle doporučení k propojení hello tooacquire hello schválení verze systému Windows Server.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Vytvoření virtuálního pevného disku kompatibilní s Azure (systémem Linux)
Tato část se zaměřuje na osvědčené postupy pro vytváření obrazem virtuálního počítače se systémem Linux pro hello Azure Marketplace. Podrobný postup najdete v části toohello následující dokumentaci: [vytvoření a nahrání virtuálního pevného disku tohoto obsahuje hello operačního systému Linux](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Vytvoření virtuálního pevného disku kompatibilní s Azure (založené na Windows)
Tato část se zaměřuje na hello kroky toocreate SKU, založené na Windows serveru pro hello Azure Marketplace.

### <a name="31-ensure-that-you-are-using-hello-correct-base-vhds"></a>3.1 Ujistěte se, že používáte hello opravte základní virtuální pevné disky
Hello operačního systému virtuálního pevného disku pro bitové kopie virtuálního počítače musí být založená na schválení Azure Základní bitová kopie obsahující systém Windows Server nebo SQL Server.

toobegin, vytvoření virtuálního počítače z jednoho z následujících bitových kopií, nacházející se v hello hello [portálu Microsoft Azure][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])
* SQL Server 2014 ([Enterprise][link-sql-2014-ent], [standardní][link-sql-2014-std], [webové] [ link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [standardní][link-sql-2012-std], [webové] [ link-sql-2012-web])

Tyto odkazy naleznete také v hello publikování portálu pod stránku hello SKU.

> [!TIP]
> Pokud používáte hello aktuální portál Azure nebo prostředí PowerShell, jsou schváleny bitové kopie systému Windows Server publikované na 8. září 2014 a novější.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 Vytvoření virtuálního počítače založené na systému Windows
Z portálu Microsoft Azure hello můžete vytvořit virtuální počítač na základě schválené základní bitové kopie v několika jednoduchých krocích. Hello Následuje přehled procesu hello:

1. Na stránce hello základní bitovou kopii, vyberte **vytvořit virtuální počítač** toobe směrované toohello nové [portálu Microsoft Azure][link-azure-portal].

    ![Kreslení][img-acom-1]
2. Přihlaste se toohello portál hello účtu Microsoft a heslo pro hello chcete toouse předplatného Azure.
3. Postupujte podle výzvy toocreate hello virtuálního počítače pomocí hello základní image, které jste vybrali. Potřebujete tooprovide název hostitele (název počítače hello), uživatelské jméno (zaregistrované jako správce) a heslo pro hello virtuálních počítačů.

    ![Kreslení][img-portal-vm-create]
4. Vyberte velikost hello toodeploy hello virtuálních počítačů:

    a.    Pokud máte v plánu toodevelop hello virtuálního pevného disku na místě, velikost hello nezáleží. Zvažte použití mezi hello menší virtuálních počítačů.

    b.    Pokud máte v plánu toodevelop hello bitové kopie v Azure, vezměte v úvahu, že pomocí jednoho z hello doporučené velikosti virtuálních počítačů pro vybranou image hello.

    c.    Informace o cenách najdete v části toohello **doporučená cenové úrovně** selektor zobrazí na portálu hello. Bude poskytovat hello tři doporučené velikosti poskytované hello vydavatele. (V tomto případě hello vydavatele je Microsoft.)

    ![Kreslení][img-portal-vm-size]
5. Nastavte vlastnosti:

    a.    Pro rychlé nasazení, můžete nechat hello výchozí hodnoty pro vlastnosti hello pod **volitelné konfiguraci** a **skupiny prostředků**.

    b.    V části **účet úložiště**, můžete volitelně vybrat účet hello úložiště, ve které hello operační systém se VHD uloží.

    c.    V části **skupiny prostředků**, můžete volitelně vybrat hello logickou skupinu, ve které tooplace hello virtuálních počítačů.
6. Vyberte hello **umístění** pro nasazení:

    a.    Pokud máte v plánu toodevelop hello virtuálního pevného disku na místě, hello umístění není důležité, protože odešlete hello image tooAzure později.

    b.    Pokud máte v plánu toodevelop hello bitové kopie v Azure, zvažte použití jeden z oblasti USA na základě Microsoft Azure hello od začátku hello. Urychluje virtuálního pevného disku hello kopírování se proces, který provádí Microsoft vaším jménem při odesílání bitové kopie pro certifikaci.

    ![Kreslení][img-portal-vm-location]
7. Klikněte na možnost **Vytvořit**. Hello virtuální počítač spustí toodeploy. V rámci minut bude mít úspěšné nasazení a můžete začít toocreate hello image pro vaše SKU.

### <a name="33-develop-your-vhd-in-hello-cloud"></a>3.3 vyvíjet svůj disk VHD v cloudu hello
Důrazně doporučujeme vývoji svůj disk VHD v cloudu hello pomocí protokolu RDP (Remote Desktop). Připojíte se tooRDP s hello uživatelské jméno a heslo zadané při zřizování.

> [!IMPORTANT]
> Pokud vyvíjíte svůj disk VHD najdete v části místní (což nedoporučujeme), [vytváření bitové kopie virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md). Stahování svůj disk VHD není nutný, pokud vyvíjíte v cloudu hello.
>
>

**Připojení přes RDP pomocí hello [portálu Microsoft Azure][link-azure-portal]**

1. Vyberte **Procházet** > **virtuálních počítačů**.
2. Otevře se okno Hello virtuálních počítačů. Ujistěte se, že virtuální počítač, který chcete tooconnect s hello běží a vyberte ho ze seznamu hello nasazené virtuální počítače.
3. Otevře se okno, které popisuje hello vybraný virtuální počítač. V horní části hello, klikněte na tlačítko **Connect**.
4. Jste výzvami tooenter hello uživatelské jméno a heslo, které jste zadali během zřizování.

**Připojení přes RDP pomocí prostředí PowerShell**

toodownload tooa místního počítače souboru vzdálené plochy, použijte hello [rutiny Get-AzureRemoteDesktopFile][link-technet-2]. V pořadí toouse této rutiny, potřebujete název hello tooknow hello služby a název hello virtuálních počítačů. Pokud jste vytvořili hello virtuálních počítačů z hello [portálu Microsoft Azure][link-azure-portal], můžete najít tyto informace v části Vlastnosti virtuálního počítače:

1. Hello portálu Microsoft Azure, vyberte **Procházet** > **virtuálních počítačů**.
2. Otevře se okno Hello virtuálních počítačů. Vyberte hello virtuální počítač, který jste nasadili.
3. Otevře se okno, které popisuje hello vybraný virtuální počítač.
4. Klikněte na **Vlastnosti**.
5. první část Hello hello název domény je název služby hello. název hostitele Hello je hello název virtuálního počítače.

    ![Kreslení][img-portal-vm-rdp]
6. soubor RDP Hello rutiny toodownload hello pro místní desktop hello vytvořit virtuální počítač toohello správce je následující.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Další informace o protokolu RDP naleznete na webu MSDN v článku hello [připojit tooan virtuálního počítače Azure pomocí protokolu RDP nebo SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Nakonfigurujte virtuální počítač a vytvořte vaše SKU**

Po hello operačního systému, který je stáhli VHD použijte Hyper-v a konfigurace virtuálního počítače toobegin, vytváření vašeho SKU. Podrobné kroky najdete na následující odkaz TechNet hello: [Hyper-v instalovat a konfigurovat virtuální počítač](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-hello-correct-vhd-size"></a>3.4 Zvolte hello správnou velikost virtuálního pevného disku
Hello operačního systému Windows virtuálního pevného disku v bitové kopii virtuálního počítače by se vytvořit jako virtuální pevný disk pevného formátu 128 GB.  

Pokud velikost fyzické hello je méně než 128 GB, musí být zhuštěných hello virtuálního pevného disku. Hello základní Windows a SQL Server bitové kopie zadaný již splňovat tyto požadavky, takže neměňte hello formátu nebo hello velikost hello získat virtuálního pevného disku.  

Datových disků může být větší než 1 TB. Při rozhodování o velikosti disku hello, mějte na paměti, že zákazníci nemůže změnit velikost virtuálních pevných disků v rámci bitovou kopii v době hello nasazení. Datový disk virtuálních pevných disků by se vytvořit jako virtuální pevný disk pevného formátu. Měly by být zhuštěných. Datové disky můžou být prázdné nebo můžou obsahovat data.

### <a name="35-install-hello-latest-windows-patches"></a>3.5 instalace nejnovějších oprav Windows hello
Hello základní Image obsahují nejnovější opravy hello až tootheir publikovaná data. Před publikováním hello operačního systému virtuálního pevného disku, které jste vytvořili, zajistěte, aby Windows Update byl spuštěn a zda všechny hello nejnovější kritický a důležité aktualizace zabezpečení byly nainstalovány.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 proveďte další úlohy konfigurace a plán, podle potřeby
Pokud není nutná další konfigurace, zvažte použití naplánovanou úlohu, která se spouští na spuštění toomake všechny poslední změny toohello virtuální počítač po jeho nasazení:

* Je úlohu hello toohave osvědčených postupů odstranit sám sebe na úspěšné provedení.
* Žádná konfigurace se spoléhají na jednotkách než jednotky jazyka C nebo D, protože se jedná hello pouze dvě jednotky, které jsou vždy zaručit tooexist. Jednotka C je hello disk operačního systému a jednotky D je hello dočasné místní disk.

### <a name="37-generalize-hello-image"></a>3.7 generalize hello image
Všechny bitové kopie v Azure Marketplace hello musí být znovu použitelné obecné způsobem. Jinými slovy musí být zobecněn hello operačního systému virtuálního pevného disku:

* Pro Windows hello obrázku musí být "Sysprep" a by mělo být provedeno žádné konfigurace, které nepodporují hello **sysprep** příkaz.
* Můžete spustit následující příkaz z hello adresáře % windir%\System32\Sysprep hello.

        sysprep.exe /generalize /oobe /shutdown

  Informace o tom, jak je toosysprep hello operační systém uvedený v kroku hello následujícím článku na webu MSDN: [vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Nasadit virtuální počítač z virtuální pevné disky
Poté, co jste nahráli virtuální pevné disky (operačního systému hello zobecněný virtuální pevný disk a nula nebo více dat na disku VHD) tooan účtu úložiště Azure, můžete zaregistrovat je jako uživatelské image virtuálního počítače. Potom můžete otestovat této bitové kopie. Všimněte si, že vzhledem k tomu, že je zobecněný virtuální pevný disk operačního systému, nelze nasadit přímo hello virtuálních počítačů tím, že poskytuje hello adresu URL VHD.

toolearn více informací o Image virtuálních počítačů, zkontrolujte hello následujících příspěvcích na blogu:

* [Image virtuálního počítače](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [Jak prostředí PowerShell bitové kopie virtuálního počítače](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [O Image virtuálních počítačů v Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-hello-necessary-tools-powershell-and-azure-cli"></a>Nastavení hello nezbytné nástrojů, prostředí PowerShell a rozhraní příkazového řádku Azure
* [Jak toosetup prostředí PowerShell](/powershell/azure/overview)
* [Jak toosetup rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 vytvořit uživatelské image virtuálního počítače
#### <a name="capture-vm"></a>Zachycení virtuálního počítače
Přečtěte si prosím hello odkazy níže uvedené pokyny k zaznamenání hello virtuálního počítače pomocí rozhraní API, Powershellu nebo Azure CLI.

* [Rozhraní API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Zobecněte Image
Přečtěte si prosím hello odkazy níže uvedené pokyny k zaznamenání hello virtuálního počítače pomocí rozhraní API, Powershellu nebo Azure CLI.

* [Rozhraní API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 nasaďte virtuální počítač z uživatelské image virtuálního počítače
toodeploy virtuální počítač z uživatelské image virtuálního počítače, můžete použít aktuální hello [portál Azure](https://manage.windowsazure.com) nebo prostředí PowerShell.

**Nasadit virtuální počítač z aktuální portál Azure hello**

1. Přejděte příliš**nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**.

    ![Kreslení][img-manage-vm-new]
2. Přejděte příliš**Moje image**, a pak vyberte hello image virtuálního počítače, ze které toodeploy virtuálního počítače:

   1. Platím zvýšené pozornosti toowhich bitové kopie můžete vybrat, protože hello **Moje image** zobrazení obsahuje bitové kopie operačního systému a Image virtuálních počítačů.
   2. Prohlížení hello počet disků může pomoct určit, jaký typ obrázku nasazujete, protože většina hello Image virtuálních počítačů používat více než jeden disk. Je však stále možné toohave image virtuálního počítače s pouze jednoho operačního systému disku, což by pak znamenalo **počet disků** nastavit too1.

      ![Kreslení][img-manage-vm-select]
3. Postupujte podle Průvodce vytvořením virtuálního počítače hello a zadejte název virtuálního počítače hello, velikost virtuálního počítače, umístění, uživatelské jméno a heslo.

**Nasadit virtuální počítač z prostředí PowerShell**

toodeploy velký virtuální počítač z hello zobecněn image virtuálního počítače, které jsou právě vytvořili, můžete použít následující rutiny hello.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> O další pomoc naleznete [Poradce při potížích s běžné problémy došlo při vytváření virtuálního pevného disku].
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Získat certifikační pro bitové kopie virtuálního počítače
dalším krokem Hello při přípravě image virtuálních počítačů pro hello Azure Marketplace je toohave, které ho certifikaci.

Tento proces zahrnuje spuštění nástroje speciální certifikační, odesílání toohello výsledky ověření hello kontejner Azure, kde jsou umístěny virtuální pevné disky, přidání nabídku, definování vaší SKU a odesílání virtuálního počítače bitové kopie pro certifikaci.

### <a name="51-download-and-run-hello-certification-test-tool-for-azure-certified"></a>5.1 stažení a spuštění hello nástroj pro testování certifikační pro certifikaci Azure
spuštění nástroje certifikační Hello v spuštěný virtuální počítač, zajištěného z vaší uživatelské image virtuálního počítače, tooensure, který hello image virtuálního počítače je kompatibilní s Microsoft Azure. Ověří, že byly splněny hello pokyny a požadavky týkající se přípravy svůj disk VHD. výstup Hello nástroje hello je zpráva o kompatibilitě, které musí být nahrán na hello publikování portál při požadavku certifikační.

Nástroj certifikační Hello lze použít s Windows a virtuální počítače s Linuxem. Připojení na základě tooWindows virtuálních počítačů pomocí prostředí PowerShell a připojí virtuální počítače tooLinux prostřednictvím SSH.Net:

1. První, stáhněte si nástroj Certifikační hello v hello [společnosti Microsoft stažení][link-msft-download].
2. Otevřete nástroj Certifikační hello a pak klikněte na hello **spustit nový Test** tlačítko.
3. Z hello **testování informace** obrazovky, zadejte název pro test hello spustit.
4. Zvolte, jestli váš virtuální počítač používá Linux nebo Windows. V závislosti na tom, který zvolíte vyberte následující možnosti hello.

### <a name="connect-hello-certification-tool-tooa-linux-vm-image"></a>**Připojit hello certifikační nástroj tooa bitové kopie virtuálního počítače s Linuxem**
1. Režim ověřování SSH vyberte hello: heslo nebo klíč souboru.
2. Pokud používáte ověřování pomocí hesla, zadejte název hello systému DNS (Domain Name), uživatelské jméno a heslo.
3. Pokud používáte ověřování soubor klíče, zadejte název DNS hello, uživatelské jméno a umístění privátního klíče.

   ![Ověřování hesla bitové kopie virtuálních počítačů Linux][img-cert-vm-pswd-lnx]

   ![Soubor klíče ověřování bitové kopie virtuálních počítačů Linux][img-cert-vm-key-lnx]

### <a name="connect-hello-certification-tool-tooa-windows-based-vm-image"></a>**Připojení bitové kopie hello certifikační nástroj tooa virtuálních počítačů na bázi Windows**
1. Zadejte hello plně kvalifikovaný název DNS virtuálního počítače (například MyVMName.Cloudapp.net).
2. Zadejte hello uživatelské jméno a heslo.

   ![Ověřování hesla bitové kopie systému Windows virtuálního počítače][img-cert-vm-pswd-win]

Po výběru hello správnými možnostmi pro bitové kopie virtuálního počítače založené na systému Windows nebo Linux, vyberte **Test připojení** tooensure SSH.Net nebo prostředí PowerShell má platné připojení k pro účely testování. Po vytvoření připojení, vyberte **Další** toostart hello testu.

Po dokončení testu hello obdržíte hello výsledky (Pass nebo selhání nebo upozornění) pro každý prvek testu.

![Testovací případy bitové kopie virtuálních počítačů Linux][img-cert-vm-test-lnx]

![Testovací případy pro Image virtuálního počítače Windows][img-cert-vm-test-win]

Pokud některý z testů hello selže, nebude musí být certifikovaná bitové kopie. Pokud k tomu dojde, zkontrolujte požadavky na hello a proveďte potřebné změny.

Po hello automatizovaných testů, zobrazí se výzva tooprovide další vstup na image virtuálního počítače prostřednictvím dotazník obrazovky.  Dokončení hello otázky a potom vyberte **Další**.

![Dotazník nástroj certifikace][img-cert-vm-questionnaire]

![Dotazník nástroj certifikace][img-cert-vm-questionnaire-2]

Po dokončení hello dotazník, můžete zadat další informace, jako jsou informace o přístupu SSH pro hello bitové kopie virtuálního počítače s Linuxem a vysvětlení všech neúspěšných testů. Můžete stáhnout hello výsledků testů a soubory protokolu pro hello provést testovací případy v přidání tooyour odpovědi toohello dotazníku. Výsledky hello uložit v hello stejném kontejneru jako virtuální pevné disky.

![Uložit certifikační výsledky testů][img-cert-vm-results]

### <a name="52-get-hello-shared-access-signature-uri-for-your-vm-images"></a>5.2 získáte sdílený přístupový podpis hello identifikátor URI pro vaše Image virtuálních počítačů
Během procesu publikování hello zadejte identifikátory URI hello (Identifier), které vést tooeach hello virtuální pevné disky, které jste vytvořili pro vaše SKU. Microsoft musí během procesu certifikace hello přístup toothese virtuální pevné disky. Proto musíte toocreate sdílený přístupový podpis identifikátor URI pro každý virtuální pevný disk. Toto je identifikátor URI, který by měly být zadány ve hello **bitové kopie** ve hello publikování portálu.

sdílený přístupový podpis Hello URI vytvořen by měl splňovat toohello následující požadavky:

* Při generování sdílený přístupový podpis identifikátory URI pro virtuální pevné disky, je dostatečné oprávnění seznamu a pro čtení. Neposkytujte přístup pro psaní nebo odstranění.
* Hello trvání pro přístup musí být minimálně týdny tří (3) ze při vytvoření hello sdílený přístupový podpis identifikátor URI.
* toosafeguard pro čas UTC, vyberte hello den před hello aktuální datum. Například pokud je aktuální datum hello 6. října 2014, vyberte 5/10/2014.

SAS adresa URL může být generována v několika způsoby tooshare svůj disk VHD pro Azure Marketplace.
Následující jsou hello 3 doporučených nástrojů:

1.  Azure Storage Explorer
2.  Průzkumník úložišť Microsoft
3.  Azure CLI

**Azure Storage Explorer (doporučeno pro uživatele systému Windows)**

Následující části jsou hello kroky pro vytvoření adresy URL SAS pomocí Azure Storage Explorer

1. Stáhněte si [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) na webu CodePlex. Přejděte příliš[Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) a klikněte na tlačítko **"Server"**.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Stáhněte si [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) a po rozbalení ji nainstalovat.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Po instalaci, otevřete aplikaci hello.
4. Klikněte na tlačítko **přidejte účet**.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Zadejte název účtu úložiště hello, klíč účtu úložiště a úložiště koncové body doménu. Toto je účet úložiště hello ve vašem předplatném Azure, kde mají uchovávat svůj disk VHD na portálu Azure.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Po připojení Azure Storage Explorer tooyour účet konkrétní úložiště, zahájí se zobrazují všechny hello obsahuje v rámci účtu úložiště hello. Vyberte hello kontejner, kam jste zkopírovali soubor VHD disku operačního systému hello (také datových disků Pokud jsou k dispozici pro váš scénář).

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Po výběru hello kontejner objektů blob, spustí Azure Storage Explorer zobrazující hello soubory hello kontejneru. Vyberte soubor bitové kopie hello (VHD), který potřebuje toobe odeslána.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Po výběru souboru VHD hello v hello kontejneru, klikněte na hello **zabezpečení** kartě.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  V hello **zabezpečení kontejneru objektů Blob** dialogové okno, ponechejte výchozí nastavení hello na hello **úroveň přístupu** a pak klikněte **sdílené přístupové podpisy** kartě.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. Postupujte podle kroků hello toogenerate sdílený přístupový podpis identifikátor URI pro hello .vhd image:

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Přístup povolen ze:** toosafeguard pro čas UTC, vyberte hello den před hello aktuální datum. Například pokud je aktuální datum hello 6. října 2014, vyberte 5/10/2014.

    b. **Umožnit přístup:** vyberte datum, které je minimálně 3 týdny po hello **přístup povolen ze** datum.

    c. **Akce povolené:** vyberte hello **seznamu** a **čtení** oprávnění.

    d. Pokud jste zvolili souboru VHD správně, pak se zobrazí v souboru **Blob název tooaccess** s příponou VHD.

    e. Klikněte na tlačítko **generování podpisu**.

    f. V **vygenerovat sdílený přístup podpis identifikátor URI tohoto kontejneru**, zkontrolujte následující jako zvýrazněný výš hello:

       - Ujistěte se, že název souboru bitové kopie a **"VHD"** jsou v hello identifikátor URI.
       - Na konci hello hello podpisu, ujistěte se, že **"= rl"** se zobrazí. To znamená, že přístup pro čtení a seznamu zadaná úspěšně.
       - Uprostřed hello podpis, ujistěte se, že **"sr = c"** se zobrazí. Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru

11. tooensure, který hello vygenerovat sdílený přístup podpis URI funguje, klikněte na tlačítko **Test v prohlížeči**. By se měl spustit proces stahování hello.

12. Zkopírujte hello sdílený přístupový podpis identifikátor URI. Toto je identifikátor URI toopaste hello do hello publikování portálu.

13. Opakujte kroky 6-10 pro každý virtuální pevný disk v hello SKU.

**Microsoft Azure Storage Explorer (Windows nebo MAC/Linux)**

Následující části jsou hello kroky pro vytvoření adresy URL SAS pomocí Microsoft Azure Storage Explorer

1.  Stáhněte si Microsoft Azure Storage Explorer formuláře [http://storageexplorer.com/](http://storageexplorer.com/) webu. Přejděte příliš[Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) a klikněte na tlačítko **"Stáhnout pro systém Windows"**.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Po instalaci, otevřete aplikaci hello.

3.  Klikněte na tlačítko **přidejte účet**.

4.  Konfigurace odběru Microsoft Azure Storage Explorer tooyour přihlašovací účet tooyour

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Přejděte toostorage účtu a vyberte možnost hello kontejneru

6.  Vyberte **"Get přístupový podpis sdílenou složku..."** pomocí klikněte pravým tlačítkem z hello **kontejneru**

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Čas spuštění aktualizace, čas vypršení platnosti a oprávnění podle následující

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Čas spuštění:** toosafeguard pro čas UTC, vyberte hello den před hello aktuální datum. Například pokud je aktuální datum hello 6. října 2014, vyberte 5/10/2014.

    b.  **Čas vypršení platnosti:** vyberte datum, které je minimálně 3 týdny po hello **čas spuštění** datum.

    c.  **Oprávnění:** vyberte hello **seznamu** a **čtení** oprávnění

8.  Zkopírujte URI sdílený přístupový podpis kontejneru

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Adresa URL generovaného SAS pro kontejner úroveň a nyní potřebujeme tooadd název disku VHD v ní.

    Formát adresy URL úrovně SAS kontejneru:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Za hello název kontejneru v adrese URL SAS jak je uvedeno níže vložit název disku VHD`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Příklad:

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    TestRGVM201631920152.vhd je hello název virtuálního pevného disku, pak bude mít adresu URL SAS virtuálního pevného disku`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Ujistěte se, že název souboru bitové kopie a **"VHD"** jsou v hello identifikátor URI.
    - Uprostřed hello podpis, ujistěte se, že **"sp = rl"** se zobrazí. To znamená, že přístup pro čtení a seznamu zadaná úspěšně.
    - Uprostřed hello podpis, ujistěte se, že **"sr = c"** se zobrazí. Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru

9.  tooensure, který hello generovaného sdílený přístupový podpis URI funguje, otestovat ji v prohlížeči. By se měl spustit proces stahování hello

10. Zkopírujte hello sdílený přístupový podpis identifikátor URI. Toto je identifikátor URI toopaste hello do hello publikování portálu.

11. Tento postup opakujte pro každý virtuální pevný disk v hello SKU.

**Rozhraní příkazového řádku Azure (doporučeno pro jiný systém než Windows & nepřetržité integrace)**

Následující části jsou hello kroky pro vytvoření adresy URL SAS pomocí rozhraní příkazového řádku Azure

1.  Stáhněte si Microsoft Azure CLI z [zde](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). Můžete také najít jiné odkazy pro  **[Windows](http://aka.ms/webpi-azure-cli)**  a  **[MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Po jeho stažení, nainstalujte prosím

3.  Vytvořte soubor prostředí PowerShell s následujícím kódem a uložit na místní

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Aktualizovat hello následující parametry ve výše

    a. **`<StorageAccountName>`**: Zadejte název účtu úložiště

    b. **`<Storage Account Key>`**: Zadejte klíč účtu úložiště

    c. **`<Permission Start Date>`**: toosafeguard pro čas UTC, vyberte hello den před hello aktuální datum. Například pokud hello aktuální datum následuje 26 října 2016, pak hodnota by měla být 10/25/2016

    d. **`<Permission End Date>`**: Vyberte datum, které je minimálně 3 týdny po hello **počáteční datum**. Měla by být hodnota **11/02/2016**.

    Následuje příklad kódu hello po aktualizaci správné parametry

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  Otevřete editor prostředí Powershell s režimem "Spustit jako správce" a otevřete soubor v kroku #3.

5.  Spuštění hello skriptu a zajistí, že hello SAS URL pro kontejner úrovně přístupu

    Následující bude výstup hello hello podpis SAS a zkopírujte části hello zvýrazněných v programu Poznámkový blok

    ![Kreslení](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Nyní budete mít úroveň kontejneru SAS adresa URL a je třeba název disku VHD tooadd v ní.

    Kontejner úrovně SAS URL #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  Vložit název disku VHD po hello název kontejneru v adrese URL SAS, jak je uvedeno níže`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Příklad:

    TestRGVM201631920152.vhd je hello název virtuálního pevného disku, pak bude mít adresu URL SAS virtuálního pevného disku

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Ujistěte se, že název souboru obrázku a "VHD" jsou hello identifikátor URI.
    -   Uprostřed hello podpis, ujistěte se, že se zobrazí "sp = rl". To znamená, že přístup pro čtení a seznamu zadaná úspěšně.
    -   Uprostřed hello podpis, ujistěte se, že "sr = c" se zobrazí. Tento příklad ukazuje, abyste měli přístup na úrovni kontejneru

8.  tooensure, který hello generovaného sdílený přístupový podpis URI funguje, otestovat ji v prohlížeči. By se měl spustit proces stahování hello

9.  Zkopírujte hello sdílený přístupový podpis identifikátor URI. Toto je identifikátor URI toopaste hello do hello publikování portálu.

10. Tento postup opakujte pro každý virtuální pevný disk v hello SKU.


### <a name="53-provide-information-about-hello-vm-image-and-request-certification-in-hello-publishing-portal"></a>5.3 poskytují informace o hello image virtuálního počítače a požádat o certifikaci v hello publikování portálu
Po vytvoření nabídku a SKU, měli byste zadat podrobnosti bitové kopie hello přidružené k této SKU:

1. Přejděte toohello [publikování portál][link-pubportal]a pak se přihlaste pomocí účtu prodejce.
2. Vyberte hello **Image virtuálních počítačů** kartě.
3. identifikátor Hello uvedené v hello horní části stránky hello je ve skutečnosti hello nabídka identifikátor a není identifikátor SKU hello.
4. Zadat vlastnosti hello pod hello **SKU** části.
5. V části **operační systém řady**, klikněte na typ operačního systému hello přidružené hello operačního systému virtuálního pevného disku.
6. V hello **operačního systému** pole, popisují hello operačního systému. Vezměte v úvahu formátu například řada operačního systému, typ, verze a aktualizace. Příkladem může být "Windows Server Datacenter 2014 R2."
7. Vyberte si toosix doporučené velikosti virtuálních počítačů. Toto jsou doporučení, které získat zobrazených toohello zákazníka v hello cenová úroveň okno v hello portálu Azure, když se rozhodnete toopurchase a nasazení bitové kopie. **Jsou to jenom doporučení. Hello zákazníka je možné tooselect jakékoli velikosti virtuálních počítačů, která bude vyhovovat hello disky zadané v bitové kopii.**
8. Zadejte verzi hello. pole verze Hello zapouzdří sémantickou verzi tooidentify hello produkt a jeho aktualizací:
   * Verze by měl být ve formátu hello X.Y.Z, kde X, Y a jsou celá čísla.
   * Bitové kopie v různých SKU může mít jiný hlavní verze a podverze.
   * Verze v rámci SKU by měl být pouze přírůstkové změny, které zvyšují verzi opravy hello (Z z X.Y.Z).
9. V hello **URL virtuálního pevného disku operačního systému** zadejte hello sdílený přístupový podpis URI vytvořený pro hello operační systém virtuálního pevného disku.
10. Pokud existují datových disků přidružených tento SKU, vyberte hello logické jednotky číslo (LUN) toowhich chcete tato data toobe disku připojené po nasazení.
11. V hello **LUN X virtuálního pevného disku URL** zadejte hello sdílený přístupový podpis URI vytvořený pro hello první data virtuálního pevného disku.

    ![Kreslení](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Běžné problémy SAS URL & opravy

|Problém|Zpráva o selhání|Oprava|Dokumentace k propojení|
|---|---|---|---|
|Chyba při kopírování bitové kopie - "?" nebyl nalezen v adrese url SAS|Chyba: Kopírování bitové kopie. Nebylo možné toodownload blob pomocí zadaný identifikátor Uri pro SAS.|Aktualizace hello SAS Url použití doporučuje nástroje|[https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Chyba při kopírování bitové kopie - parametry "st" a "se" není v adrese url SAS|Chyba: Kopírování bitové kopie. Nebylo možné toodownload blob pomocí zadaný identifikátor Uri pro SAS.|Aktualizace hello SAS adresa Url s počátečním a koncovým datem na něm|[https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Chyba při kopírování bitové kopie - "sp = rl" není v adrese url SAS|Chyba: Kopírování bitové kopie. Nebylo možné toodownload blob pomocí zadaný identifikátor Uri pro SAS|Aktualizace hello SAS Url s oprávněními nastavenými jako "Číst" a "seznamu|[https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Chyba při kopírování bitové kopie - SAS url obsahovat prázdné znaky v názvu virtuálního pevného disku|Chyba: Kopírování bitové kopie. Nebylo možné toodownload blob pomocí zadaný identifikátor Uri pro SAS.|Aktualizace hello SAS Url bez mezer|[https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Chyba při kopírování bitové kopie – Chyba autorizace adres Url SAS|Chyba: Kopírování bitové kopie. Nebylo možné toodownload blob z důvodu chyby tooauthorization|Znovu vygenerovat hello SAS adresa Url|[https://Azure.microsoft.com/en-us/documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a>Další krok
Jakmile jste hotovi s podrobnostmi hello SKU, můžete přesunout dopředného toohello [marketing vodítko obsahu Azure Marketplace][link-pushstaging]. V tomto kroku hello procesu publikování zadáte hello marketingové obsahu, ceny a jiné informace potřebné před příliš**krok 3: testování virtuální počítač v pracovní nabízejí**, kde můžete testovat různé scénáře případ použití před nasazením Hello nabídka toohello Azure Marketplace pro veřejné viditelnost a nákup.  

## <a name="see-also"></a>Viz také
* [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
