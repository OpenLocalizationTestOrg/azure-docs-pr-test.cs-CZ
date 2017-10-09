


Toto téma popisuje postup:

* Vložení dat do Azure virtuální počítač (VM) při jeho zřizování.
* Načíst ho pro systém Windows a Linux.
* Použít speciální nástroje, které jsou k dispozici na některé systémy toodetect a automaticky zpracovávat vlastní data.

> [!NOTE]
> Tento článek popisuje, jak vlastních dat může pomocí virtuální počítač vytvořen s hello Azure Service Management API vložit. jak zjistit, toouse hello rozhraní API správy prostředků Azure, toosee [hello příklad šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Vložení vlastní data do virtuálního počítače Azure
Tato funkce v současné době podporuje pouze v hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-xplat-cli). Tady vytváříme `custom-data.txt` soubor, který obsahuje naše data pak vložit, v toohello virtuálních počítačů během zřizování. I když některé z možností hello může použít pro hello `azure vm create` příkaz hello následující ukazuje jeden ze způsobů velmi základní:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a>Pomocí vlastních dat ve virtuálním počítači hello
* Pokud je virtuální počítač Azure virtuálních počítačů na bázi Windows, pak hello vlastní datový soubor je uložen příliš`%SYSTEMDRIVE%\AzureData\CustomData.bin`. I když byl tootransfer kódováním base64 z místního počítače toohello hello nového virtuálního počítače, je automaticky dekódovat a dá otevřít nebo použít okamžitě.
  
  > [!NOTE]
  > Pokud existuje soubor hello, se přepíše. Hello zabezpečení v adresáři hello je nastaven příliš**systému: Úplné řízení** a **Administrators: Úplné řízení**.
  > 
  > 
* Pokud virtuální počítač Azure je virtuální počítač založený na Linuxu, hello vlastní datový soubor bude umístěn v jedna z následujících hello umístí v závislosti na vaší distro. Hello data mohou být kódováním base64, proto může třeba toodecode hello data nejdřív:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Init cloudu v Azure
Pokud virtuální počítač Azure z image Ubuntu nebo CoreOS, můžete použít CustomData toosend cloudu config toocloud-init. Nebo pokud vlastní datový soubor skriptu, pak cloudu init jednoduše ho provést.

### <a name="ubuntu-cloud-images"></a>Ubuntu cloudu obrázků
Ve většině Image Azure Linux, by upravit "/ etc/waagent.conf" tooconfigure hello dočasných prostředků disku a odkládací soubor. V tématu [Azure Linux Agent uživatelská příručka](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace.

Ale na Image cloudu Ubuntu hello, musíte použít cloudové init tooconfigure hello prostředků disku (tj. disk hello "dočasné") a velikost odkládacího souboru oddílu. Hello následující stránky na stránkách wiki Ubuntu hello další podrobnosti najdete v části: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Další kroky: pomocí init cloudu
Další informace najdete v tématu hello [cloudu init dokumentaci Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Přidání Role referenční dokumentace rozhraní API REST služby správy](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Rozhraní příkazového řádku Azure](https://github.com/Azure/azure-xplat-cli)

