



V závislosti na prostředí a možnosti můžete vytvořit skript hello všechny infrastruktury hello clusteru, včetně hello virtuální síť Azure, účty úložiště, cloudové služby, řadič domény, vzdálení nebo místní databáze SQL, hlavního uzlu a další clusteru uzly. Hello skriptu můžete alternativně použít existující infrastrukturu Azure a vytvořit pouze hello uzly clusteru prostředí HPC.

Základní informace o plánování clusteru služby HPC Pack, najdete v části hello [období pro vyzkoušení produktu a plánování](https://technet.microsoft.com/library/jj899596.aspx) a [Začínáme](https://technet.microsoft.com/library/jj899590.aspx) obsahu v hello knihovně TechNet HPC Pack 2012 R2.

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**: předplatné můžete použít v obou hello globální Azure nebo službu Azure China. Vaše předplatné omezení ovlivnit hello počet a typ uzly clusteru, který můžete nasadit. Informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../articles/azure-subscription-service-limits.md).
* **Klientský počítač s Windows v prostředí Azure PowerShell 0.8.10 nebo později nainstalovaný a nakonfigurovaný**: najdete v části [Začínáme s Azure PowerShell](/powershell/azureps-cmdlets-docs) pro instalaci pokyny a kroky tooconnect tooyour předplatného Azure.
* **Skript nasazení HPC Pack IaaS**: Stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Zkontrolujte verzi hello hello skriptu spuštěním `New-HPCIaaSCluster.ps1 –Version`. Tento článek vychází z verze 4.5.2 hello skriptu.
* **Konfigurační soubor skriptu**: Vytvořte soubor XML, že skript hello používá clusteru HPC tooconfigure hello. Informace a příklady naleznete v částech později v tomto článku a hello souboru Manual.rtf, který doprovází skript nasazení hello.

## <a name="syntax"></a>Syntaxe
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Spusťte skript hello jako správce.
> 
> 

### <a name="parameters"></a>Parametry
* **ConfigFile**: Určuje cestu souboru hello hello konfigurační soubor clusteru HPC toodescribe hello. Další informace naleznete v o hello konfigurační soubor v tomto tématu nebo v souboru hello Manual.rtf ve složce hello obsahující hello skriptu.
* **AdminUserName**: Určuje hello uživatelské jméno. Pokud hello domény doménové struktury je vytvořen skriptem hello, stane hello uživatelské jméno místního správce pro všechny virtuální počítače a jméno správce domény hello. Pokud hello domény doménové struktury již existuje, určuje uživatele domény hello jako místní správce uživatele název tooinstall HPC Pack hello.
* **AdminPassword**: Určuje heslo správce hello. Pokud není zadaný v hello příkazového řádku, vyzve vás hello skriptu tooinput hello heslo.
* **HPCImageName** (volitelné): Určuje název bitové kopie virtuálních počítačů HPC Pack hello používá clusteru HPC toodeploy hello. Musí to být image poskytovaný společností Microsoft HPC Pack z hello Azure Marketplace. Pokud není zadaný hello (doporučená obvykle), skript zvolí hello nejnovější publikovaná [image HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). Hello nejnovější image založena na Windows Server 2012 R2 Datacenter s HPC Pack 2012 R2 Update 3 nainstalován.
  
  > [!NOTE]
  > Nasazení se nezdaří, pokud neurčíte platnou bitovou kopií HPC Pack.
  > 
  > 
* **Soubor protokolu** (volitelné): Určuje cestu souboru protokolu nasazení hello. Pokud není zadaný, skript hello vytvoří soubor protokolu v adresáři temp hello hello počítače se systémem hello skriptu.
* **Platnost** (volitelné): potlačí všechny výzvy potvrzení hello.
* **NoCleanOnFailure** (volitelné): Určuje, že hello virtuálních počítačích Azure, která nejsou nasazena úspěšně neodeberou. Odebrat tyto virtuální počítače ručně před opětovným spuštěním hello skriptu toocontinue hello nasazení nebo hello nasazení může selhat.
* **PSSessionSkipCACheck** (volitelné): pro všechny cloudové služby s virtuálními počítači nasadit pomocí tohoto skriptu certifikát podepsaný svým držitelem je automaticky generován Azure a hello všechny virtuální počítače v cloudové službě hello použít tento certifikát jako výchozí Windows hello Vzdálená správa systému Windows (WinRM) certifikát. Funkce toodeploy HPC v těchto virtuálních počítačích Azure, skript hello ve výchozím nastavení dočasně nainstaluje tyto certifikáty v místním počítači hello\\úložiště Důvěryhodné kořenové certifikační autority hello klientský počítač toosuppress hello "není důvěryhodné certifikační Autority" zabezpečení Při provádění skriptu došlo k chybě. Hello certifikáty jsou odebrány, po dokončení skriptu hello. Pokud je tento parametr zadán, hello certifikáty nejsou nainstalované v klientském počítači hello a potlačeno hello upozornění zabezpečení.
  
  > [!IMPORTANT]
  > Tento parametr se nedoporučuje pro nasazení v produkčním prostředí.
  > 
  > 

### <a name="example"></a>Příklad
Hello následující příklad vytvoří cluster služby HPC Pack pomocí konfiguračního souboru *MyConfigFile.xml*a určuje přihlašovací údaje správce pro instalaci clusteru hello.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Další aspekty
* skript Hello Volitelně můžete umožnit odesílání úlohy prostřednictvím webového portálu HPC Pack hello nebo hello HPC Pack REST API.
* Hello skriptu můžete případně spustit vlastní skripty před a po konfiguraci hlavního uzlu hello Chcete-li další software tooinstall nebo konfigurovat další nastavení.

## <a name="configuration-file"></a>Konfigurační soubor
konfigurační soubor Hello hello nasazení skriptu je soubor XML. soubor schématu Hello HPCIaaSClusterConfig.xsd je ve složce skriptu nasazení HPC Pack IaaS hello. **IaaSClusterConfig** je hello kořenový element hello konfiguračního souboru, který obsahuje podrobný popis naleznete v souboru hello Manual.rtf ve složce skriptu nasazení hello hello podřízené elementy.

