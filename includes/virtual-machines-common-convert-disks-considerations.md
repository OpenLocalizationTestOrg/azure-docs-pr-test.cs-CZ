
* Hello převod vyžaduje restartování hello virtuálních počítačů, takže naplánovat hello migraci virtuálních počítačů během existující údržby. 

* Převod Hello není reverzibilního. 

* Být, že conversion tootest hello. Migrujte testovací virtuální počítač, před provedením migrace hello v produkčním prostředí.

* Při převodu hello navrácení hello virtuálních počítačů. Hello virtuálního počítače obdrží novou IP adresu, když se spustí po převodu hello. V případě potřeby můžete [přiřadit statickou IP adresu](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) toohello virtuálních počítačů.

* Hello původní virtuální pevné disky a účet úložiště hello používá hello virtuálních počítačů před převodem nebudou odstraněny. Budou pokračovat v práci tooincur poplatky. tooavoid se účtují pro tyto artefakty odstranit původní BLOB VHD hello po ověření, že dokončení převodu hello.
