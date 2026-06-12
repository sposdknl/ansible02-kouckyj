# MP-Automatizovaná instalace a správa Grafana pomocí Ansible

Některé části mi byly poskytnuty a všechny převzaté části budou zmíněny v textu práce.

## Závislosti

1. **Vagrant 2.4.9** – dostupné online na: https://developer.hashicorp.com/vagrant/install
2. **VirtualBox 7.2.4** – dostupné online na: https://www.virtualbox.org/wiki/Downloads
3. **Git 2.52.0** – dostupné online na: https://git-scm.com/install/

## Návod ke spuštění

### 1. Stažení projektu
Stažení projektu pomocí příkazu `git clone` v prázdném adresáři:

```bash
cd /cesta-k-vasemu-adresari/
git clone [https://github.com/JanMizera/MP-Automatizovana-instalace-a-sprava-Grafana-pomoci-Ansible.git](https://github.com/JanMizera/MP-Automatizovana-instalace-a-sprava-Grafana-pomoci-Ansible.git)
```

### 2. Spuštění projektu
Spuštění infrastruktury pomocí příkazu:

```bash
vagrant up
```

**Upozornění:** V případě zaseknutí při práci s SSH klíči je potřeba otevřít danou VM přes GUI VirtualBoxu. Pokud dojde k timeoutu, je potřeba prostředí smazat a spustit znovu:

```bash
vagrant destroy -f
vagrant up
```

### 3. Konfigurace serverů
Po dokončení běhu je dostupné:
* **Zabbix GUI:** http://localhost:8002 (Uživatel: `Admin`, Heslo: `Zabbix`)
* **Grafana GUI:** http://localhost:3000 (Uživatel: `admin`, Heslo: `GrafanaUltimateAdminPassword123`)

Pro samotnou konfiguraci se připojte na **Bastion-SRV03** přes Vagrant:

```bash
vagrant ssh bastion-srv03
```

Následně přejděte do adresáře projektu a spusťte playbooky pomocí Ansible:

```bash
cd /opt/repo
ansible-playbook configure_zabbix_server.yml
sudo ansible-playbook configure_grafana_server.yml
```

### 4. Zobrazení výsledků
* Po úspěšném provedení první role uvidíte v GUI Zabbixu přidané hosty.
* Po úspěšném provedení druhé role uvidíte v GUI Grafany přidané dashboardy, na kterých se zobrazují data ze Zabbixu.

---

## Předpoklady a řešení problémů s boxem

Při prvních spuštěních projektu nastal problém, kdy Vagrant nebyl schopen nainstalovat box `bento/ubuntu-24.04` z Vagrant cloudu. Nyní by již automatický import měl fungovat, ale v případě potíží postupujte následovně:

### 1. Ruční stažení Vagrant Boxu
1. **Stáhněte** box `bento/ubuntu-24.04` (verze pro `amd64` / VirtualBox) z oficiálního Vagrant Cloudu.
2. **Přejmenujte** soubor na: `bento-ubuntu-24.04-virtualbox.box`.
3. **Přesuňte** soubor do kořenového adresáře tohoto projektu.

### 2. Import Boxu do Vagrantu
Spusťte příkaz pro ruční přidání boxu do lokální Vagrant cache z kořenového adresáře:

```bash
vagrant box add bento/ubuntu-24.04 bento-ubuntu-24.04-virtualbox.box --provider virtualbox
```

### 3. Spuštění projektu
Nyní pokračujte podle standardního návodu výše (krok `vagrant up`).