# OSY: Automatická registrace Debianu 13 do Zabbixu

![Virtuální server s Debianem 13 propojený se Zabbix monitoringem](Images/debian13-zabbix-agent2-hero.webp)

Toto je šablona zadání pro výuku předmětu OSY na [osy.sposdk.cz](https://osy.sposdk.cz/). Z této šablony vznikne váš vlastní žákovský repozitář. Pracujte a odevzdávejte změny v něm.

## Cíl úlohy

Spusťte pomocí Vagrantu virtuální stroj s **Debianem 13**. Nainstalujte na něj **Zabbix Agent 2 verze 8.0 LTS** a nastavte jeho automatickou registraci do svého Zabbix serveru (appliance). Výsledkem má být funkční, monitorovaný host s vaším jedinečným názvem.

## Požadavky

1. Vytvořte v kořeni svého repozitáře `Vagrantfile` pro Debian 13. Jako výchozí bod můžete použít [`Vagrant/Vagrantfile-debian`](Vagrant/Vagrantfile-debian).
2. Přidejte virtuálnímu stroji druhou síťovou kartu do stejné interní sítě VirtualBoxu jako váš Zabbix server. Na Debianu nastavte adresu **`192.168.1.3/24`**; síť serveru z předchozí úlohy je `192.168.1.0/24`. Ověřte vzájemnou dostupnost obou strojů.
3. Připravte vlastní instalační a konfigurační skripty a zapojte je jako Vagrant provisionery. Instalace musí používat **oficiální repozitář Zabbix 8.0 pro Debian 13**, nikoli balíček starší verze nebo balíček z výchozího repozitáře Debianu.
4. Nastavte v agentovi `Server` a `ServerActive` na adresu **svého** Zabbix serveru, `HostMetadata=SPOS` a jedinečný `Hostname`. Název hosta musí zůstat stejný po restartu virtuálního stroje i po opakovaném spuštění provisioningu.
5. Ve webovém rozhraní Zabbixu vytvořte akci pro automatickou registraci aktivního agenta s podmínkou `HostMetadata=SPOS`. Akce musí hosta zařadit do skupiny, přidat mu štítek a přiřadit vhodnou šablonu pro monitoring Linuxu agentem 2.
6. Ověřte, že se host v Zabbixu skutečně objevil a že z něj přicházejí data.

## Stručná nápověda k postupu

- Projděte si ukázku v adresáři [`Example/`](Example/). Je určena pro starší Ubuntu a starší Zabbix: použijte ji jen jako inspiraci pro strukturu Vagrantfile a skriptů.
- V dokumentaci Vagrantu vyhledejte `private_network` a volbu `virtualbox__intnet`. Název interní sítě musí odpovídat síti vaší appliance.
- Na [stránce ke stažení Zabbixu](https://www.zabbix.com/download) vyberte větev **8.0 LTS**, distribuci **Debian 13** a komponentu **Agent 2**. Získané instalační kroky převeďte do skriptu. Po instalaci zkontrolujte verzi agenta a stav služby.
- V konfiguraci `/etc/zabbix/zabbix_agent2.conf` se zaměřte na parametry `Hostname`, `Server`, `ServerActive` a `HostMetadata`. Automatickou registraci zahajuje aktivní spojení agenta se serverem.
- Při hledání chyby ověřte IP adresy, spojení na server, stav služby `zabbix-agent2` a její záznamy v systémovém žurnálu.

## Co odevzdat

V žákovském repozitáři ponechte funkční `Vagrantfile`, své instalační a konfigurační skripty a doplňte stručný `README.md` s postupem spuštění, použitou adresou Zabbix serveru a způsobem ověření. Do adresáře [`Images/`](Images/) vložte snímek obrazovky ze Zabbixu, na kterém je vidět registrovaný host s vaším jedinečným názvem a přijímaná data.

## Dokumentace

- [Zabbix 8.0: instalace z oficiálních balíčků](https://www.zabbix.com/documentation/8.0/en/manual/installation/install/packages)
- [Zabbix 8.0: automatická registrace aktivního agenta](https://www.zabbix.com/documentation/8.0/en/manual/discovery/auto_registration)
- [Zabbix 8.0: parametry konfigurace agenta 2](https://www.zabbix.com/documentation/8.0/en/manual/concepts/agent2/agent2_params)
- [Vagrant: interní síť ve VirtualBoxu](https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/networking#virtualbox-internal-network)
