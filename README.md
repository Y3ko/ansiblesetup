### Ansible Kurulumu ve WinRM Bağlantısı

Bu rehber, Ansible ile Windows makineleri yönetmek için gerekli adımları detaylı olarak açıklamaktadır.

#### 1. Ansible Kurulumu
Ansible'ı yüklemek için:
```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install -y ansible
```

#### 2. pywinrm Kurulumu
```bash
sudo apt-get install -y python-pip
pip install pywinrm
```

#### 3. Ansible Konfigürasyonu
`/etc/ansible/hosts` dosyasını düzenleyin:
```ini
[windows]
winhost ansible_host=192.168.1.100 ansible_user=Admin ansible_password=Password ansible_connection=winrm ansible_winrm_transport=basic ansible_port=5986 ansible_winrm_server_cert_validation=ignore
```
**Not:** IP adresi, kullanıcı adı ve şifre gibi bilgileri kendi bilgilerinizle değiştirin.

#### 4. Grup Değişken Dosyası Oluşturma
```bash
sudo mkdir /etc/ansible/group_vars
sudo vi /etc/ansible/group_vars/windows.yml
```
Dosya içeriği:
```yaml
ansible_user: remote_account
ansible_password: remote_password
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
```
**Not:** `remote_account` ve `remote_password` bilgilerini kendi ayarlarınıza göre düzenleyin.

#### 5. PowerShell Scripti Oluşturma
Windows makinesinde, `ConfigureRemotingForAnsible.ps1` adında bir PowerShell dosyası oluşturun ve aşağıdaki içeriği ekleyin ve daha sonra
yönetici olarak çalıştırın:
```powershell
https://raw.githubusercontent.com/rallabandisrinivas/winrm_ansible/main/README.md
```
Bu betik, Windows makinenizi Ansible ile uyumlu hale getirmek için gerekli ayarları yapar.

#### 6. PowerShell Ayarları
PowerShell'i yönetici olarak çalıştırın ve aşağıdaki komutları girin:
```powershell
Set-ExecutionPolicy remotesigned
winrm quickconfig
winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
powershell.exe -File ConfigureRemotingForAnsible.ps1
```
Bu komutlar, WinRM hizmetini başlatır ve yapılandırır.

#### 7. Test Etme
Ansible yapılandırmanızı test etmek için:
```bash
ansible all -m win_ping
ansible windows -m win_ping
```
Bu komutlar, Ansible'ın Windows makineleriyle başarılı bir şekilde iletişim kurup kurmadığını kontrol eder.

#### 8. Komutlarla İşlem Yapma
Örnek bir komut:
```bash
ansible windows -m win_shell -a "Get-Process"
```
Bu komut, uzak Windows makinesinde çalışan tüm işlemleri listeleyecektir.

#### Ek Kaynaklar
- [WinRM Kurulumu](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html)
- [Ansible Windows Belgeleri](https://docs.ansible.com/ansible/latest/user_guide/windows.html)
