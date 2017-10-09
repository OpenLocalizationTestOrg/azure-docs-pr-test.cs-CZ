## <a name="next-steps"></a><span data-ttu-id="1262a-101">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1262a-101">Next steps</span></span>

<span data-ttu-id="1262a-102">Po povolení Azure Key Vault integrace, můžete povolit šifrování SQL serveru na virtuální počítač SQL.</span><span class="sxs-lookup"><span data-stu-id="1262a-102">After enabling Azure Key Vault Integration, you can enable SQL Server encryption on your SQL VM.</span></span> <span data-ttu-id="1262a-103">Nejprve budete potřebovat toocreate asymetrický klíč v trezoru klíčů a symetrický klíč v rámci systému SQL Server na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="1262a-103">First, you will need toocreate an asymmetric key inside your key vault and a symmetric key within SQL Server on your VM.</span></span> <span data-ttu-id="1262a-104">Potom bude možné tooexecute T-SQL příkazy tooenable šifrování pro databáze a zálohování.</span><span class="sxs-lookup"><span data-stu-id="1262a-104">Then, you will be able tooexecute T-SQL statements tooenable encryption for your databases and backups.</span></span>

<span data-ttu-id="1262a-105">Existuje několik typů šifrování, které můžete využít výhod:</span><span class="sxs-lookup"><span data-stu-id="1262a-105">There are several forms of encryption you can take advantage of:</span></span>

* [<span data-ttu-id="1262a-106">Transparentní šifrování dat (šifrování TDE)</span><span class="sxs-lookup"><span data-stu-id="1262a-106">Transparent Data Encryption (TDE)</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="1262a-107">Šifrované zálohování</span><span class="sxs-lookup"><span data-stu-id="1262a-107">Encrypted backups</span></span>](https://msdn.microsoft.com/library/dn449489.aspx)
* [<span data-ttu-id="1262a-108">Šifrování na úrovni sloupce (Vymazat)</span><span class="sxs-lookup"><span data-stu-id="1262a-108">Column Level Encryption (CLE)</span></span>](https://msdn.microsoft.com/library/ms173744.aspx)

<span data-ttu-id="1262a-109">Hello následujících Transact-SQL skriptů příklady pro každé z těchto oblastí.</span><span class="sxs-lookup"><span data-stu-id="1262a-109">hello following Transact-SQL scripts provide examples for each of these areas.</span></span>

### <a name="prerequisites-for-examples"></a><span data-ttu-id="1262a-110">Předpoklady pro příklady</span><span class="sxs-lookup"><span data-stu-id="1262a-110">Prerequisites for examples</span></span>

<span data-ttu-id="1262a-111">Každý příklad vychází z hello dva požadavky: asymetrického klíče z trezoru klíčů názvem **CONTOSO_KEY** a pověření vytvořené hello integrace se službou AZURE funkci **Azure_EKM_TDE_cred**.</span><span class="sxs-lookup"><span data-stu-id="1262a-111">Each example is based on hello two prerequisites: an asymmetric key from your key vault called **CONTOSO_KEY** and a credential created by hello AKV Integration feature called **Azure_EKM_TDE_cred**.</span></span> <span data-ttu-id="1262a-112">Hello následující příkazy jazyka Transact-SQL nastavit tyto požadavky pro spuštění hello příklady.</span><span class="sxs-lookup"><span data-stu-id="1262a-112">hello following Transact-SQL commands setup these prerequisites for running hello examples.</span></span>

``` sql
USE master;
GO

sp_configure [show advanced options], 1;
GO
RECONFIGURE;
GO

-- Enable EKM provider
sp_configure [EKM provider enabled], 1;
GO
RECONFIGURE;

--create provider
CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov
FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';
GO

--create credential
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--must have sysadmin
ALTER LOGIN [TDE_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'keytestvault',  --key name
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a><span data-ttu-id="1262a-113">Transparentní šifrování dat (šifrování TDE)</span><span class="sxs-lookup"><span data-stu-id="1262a-113">Transparent Data Encryption (TDE)</span></span>

1. <span data-ttu-id="1262a-114">Vytvoření toobe přihlášení systému SQL Server používá pro šifrování TDE hello databázový stroj a pak přidejte tooit hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1262a-114">Create a SQL Server login toobe used by hello Database Engine for TDE, then add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello TDE Login tooadd hello credential for use by the
   -- Database Engine tooaccess hello key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
   GO
   ```

1. <span data-ttu-id="1262a-115">Vytvořte hello šifrovací klíč databáze, který se použije pro šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="1262a-115">Create hello database encryption key that will be used for TDE.</span></span>

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello database tooenable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a><span data-ttu-id="1262a-116">Šifrované zálohování</span><span class="sxs-lookup"><span data-stu-id="1262a-116">Encrypted backups</span></span>

1. <span data-ttu-id="1262a-117">Vytvořte toobe přihlášení systému SQL Server používá hello databázový stroj pro šifrování záloh a přidejte tooit hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1262a-117">Create a SQL Server login toobe used by hello Database Engine for encrypting backups, and add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it is encrypting hello backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello Encrypted Backup Login tooadd hello credential for use by
   -- hello Database Engine tooaccess hello key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
   GO
   ```

1. <span data-ttu-id="1262a-118">Určení šifrování pomocí hello asymetrického klíče uloženého v trezoru klíčů hello hello zálohování databáze.</span><span class="sxs-lookup"><span data-stu-id="1262a-118">Backup hello database specifying encryption with hello asymmetric key stored in hello key vault.</span></span>

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   tooDISK = N'[PATH tooBACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a><span data-ttu-id="1262a-119">Šifrování na úrovni sloupce (Vymazat)</span><span class="sxs-lookup"><span data-stu-id="1262a-119">Column Level Encryption (CLE)</span></span>

<span data-ttu-id="1262a-120">Tento skript vytvoří symetrického klíče chráněné hello asymetrický klíč v trezoru klíčů hello a potom pomocí hello symetrického klíče tooencrypt dat v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="1262a-120">This script creates a symmetric key protected by hello asymmetric key in hello key vault, and then uses hello symmetric key tooencrypt data in hello database.</span></span>

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open hello symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data tooencrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close hello symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a><span data-ttu-id="1262a-121">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1262a-121">Additional resources</span></span>

<span data-ttu-id="1262a-122">Další informace o tom, jak toouse tyto funkce šifrování, najdete v části [pomocí EKM pomocí funkcí šifrování SQL serveru](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span><span class="sxs-lookup"><span data-stu-id="1262a-122">For more information on how toouse these encryption features, see [Using EKM with SQL Server Encryption Features](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span></span>

<span data-ttu-id="1262a-123">Všimněte si, že hello postup v tomto článku předpokládá, že už máte SQL Server běžící na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="1262a-123">Note that hello steps in this article assume that you already have SQL Server running on an Azure virtual machine.</span></span> <span data-ttu-id="1262a-124">Pokud ne, najdete v části [zřízení virtuálního počítače s SQL serverem v Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="1262a-124">If not, see [Provision a SQL Server virtual machine in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="1262a-125">Další pokyny k systémem SQL Server na virtuálních počítačích Azure, najdete v části [SQL Server na virtuálních počítačích Azure přehled](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1262a-125">For other guidance on running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
