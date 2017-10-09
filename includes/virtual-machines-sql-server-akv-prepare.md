## <a name="prepare-for-akv-integration"></a>Příprava pro integrace se službou AZURE
Azure Key Vault integrace tooconfigure toouse váš virtuální počítač SQL Server existuje několik předpokladů: 

1. [Instalace prostředí Azure Powershell](#install-azure-powershell)
2. [Vytvoření služby Azure Active Directory](#create-an-azure-active-directory)
3. [Vytvoření trezoru klíčů](#create-a-key-vault)

Hello následující části popisují tyto požadavky a hello informace, které potřebujete rutiny prostředí PowerShell hello toocollect toolater spustit.

### <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
Ujistěte se, že máte nainstalovanou hello nejnovější sadu Azure PowerShell SDK. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="create-an-azure-active-directory"></a>Vytvoření služby Azure Active Directory
Nejprve je třeba toohave [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) v rámci vašeho předplatného. Mezi řadu výhod můžete toogrant oprávnění tooyour trezor klíčů pro některé uživatele a aplikace.

V dalším kroku zaregistrujte aplikaci v AAD. Tím získáte účet instanční objekt, který má přístup tooyour trezoru klíčů který potřebovat virtuálního počítače. V článku hello Azure Key Vault, můžete najít tyto kroky v hello [zaregistrovat aplikaci s Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) oddílu, nebo můžete zobrazit kroky hello se snímky obrazovky v hello **získat identity pro aplikace hello část** z [tomto příspěvku na blogu](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Před dokončením těchto kroků, mějte na paměti, je nutné, aby toocollect hello následující informace během této registrace, který je potřeba později, když povolíte na virtuální počítač SQL Azure Key Vault integrace.

* Po přidání aplikace hello najde hello **ID klienta** na hello **konfigurace** kartě.   ![ID klienta Azure Active Directory](./media/virtual-machines-sql-server-akv-prepare/aad-client-id.png)
  
    ID klienta Hello je přiřazen novější toohello **$spName** parametr (Service Principal name) ve tooenable skript prostředí PowerShell hello Azure Key Vault integrace. 
* Navíc při provádění těchto kroků při vytváření klíče zkopírujte hello tajný klíč pro klíč znázorněné v hello následující snímek obrazovky. Tento klíč tajný klíč je přiřazen novější toohello **$spSecret** parametr (Service Principal tajný klíč) ve skriptu PowerShell hello.  
    ![Tajný klíč služby Azure Active Directory](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)
* Je nutné autorizovat tento nový klient ID toohave hello následující oprávnění k přístupu: **šifrování**, **dešifrovat**, **wrapKey**, **unwrapKey**, **přihlašovací**, a **ověřte**. To lze provést pomocí hello [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) rutiny. Další informace najdete v části [autorizovat hello aplikace toouse hello klíče nebo tajného klíče](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
Pořadí toouse Azure Key Vault toostore hello klíče, které budete používat pro šifrování v virtuálního počítače je nutné trezoru klíčů tooa přístup. Pokud již jste nenastavili trezoru klíčů, vytvořte ho pomocí následujících kroků hello v hello [Začínáme s Azure Key Vault](../articles/key-vault/key-vault-get-started.md) tématu. Před dokončením těchto kroků, Všimněte si, že některé informace, které budete potřebovat toocollect během toto nastavení, je potřeba později když povolíte na virtuální počítač SQL Azure Key Vault integrace.

Když získáte toohello vytvořit krok trezoru klíčů, vrátí Poznámka hello **vaultUri** vlastnost, která je adresa URL trezoru klíčů hello. V zadané v tomto kroku níže uvedeném příkladu hello název trezoru klíčů hello je ContosoKeyVault, proto hello URL trezoru klíčů by https://contosokeyvault.vault.azure.net/.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Adresa URL trezoru klíčů Hello je přiřazen novější toohello **$akvURL** parametr v tooenable skript prostředí PowerShell hello Azure Key Vault integrace.

