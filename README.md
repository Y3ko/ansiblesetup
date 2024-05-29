### Ansible Kurulumu ve WinRM Bağlantısı

Bu rehber, Windows makinelerde Ansible kurulumunu ve WinRM bağlantısını ayarlamayı içermektedir.

#### 1. Ansible Kurulumu
Ansible'ı Linux veya macOS işletim sistemine yüklemek için:
```bash
sudo apt update
sudo apt install ansible -y
```

#### 2. Windows Ayarları
PowerShell'i yönetici olarak çalıştırın ve WinRM'i etkinleştirin:
```powershell
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="*"}
```

#### 3. Ansible Konfigürasyonu
Windows makinenizi `inventory` dosyasına ekleyin:
```ini
[windows]
winhost ansible_host=192.168.1.100 ansible_user=Admin ansible_password=Password ansible_connection=winrm ansible_winrm_server_cert_validation=ignore
```

#### 4. Test Etme
Konfigürasyonu test edin:
```bash
ansible windows -m win_ping
```

#### 5. Ek Kaynaklar
- [WinRM Kurulumu](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html)
- [Ansible Windows Belgeleri](https://docs.ansible.com/ansible/latest/user_guide/windows.html)

---
