# OSY: Automatická registrace Debianu 13 do Zabbixu

![Virtuální server s Debianem 13 propojený se Zabbix monitoringem](Images/debian13-zabbix-agent2-hero.webp)

Toto je šablona zadání pro výuku předmětu OSY na [osy.sposdk.cz](https://osy.sposdk.cz/). Z této šablony vznikne váš vlastní žákovský repozitář. Pracujte a odevzdávejte změny v něm.

## Cíl úlohy

Spusťte pomocí Vagrantu virtuální stroj s **Debianem 13**. Nainstalujte na něj **Zabbix Agent 2 verze 8.0 LTS** a nastavte jeho automatickou registraci na server `enceladus.pfsense.cz`. Instalace a konfigurace agenta musí proběhnout automaticky prostřednictvím provisioningu ve `Vagrantfile`; ruční instalace uvnitř virtuálního stroje nestačí. Vzorové skripty najdete v adresáři [`Example/`](Example/). Výsledkem má být funkční, monitorovaný host s vaším jedinečným názvem.

## Požadavky

1. Vytvořte v kořeni svého repozitáře `Vagrantfile` pro Debian 13. Jako výchozí bod můžete použít [`Vagrant/Vagrantfile-debian`](Vagrant/Vagrantfile-debian).
2. Připravte vlastní instalační a konfigurační skripty a zapojte je jako Vagrant provisionery. Instalace musí používat **oficiální repozitář Zabbix 8.0 pro Debian 13**, nikoli balíček starší verze nebo balíček z výchozího repozitáře Debianu.
3. Nastavte v agentovi `Server` a `ServerActive` na `enceladus.pfsense.cz`, `HostMetadata=SPOS` a jedinečný `Hostname`. Název hosta musí zůstat stejný po restartu virtuálního stroje i po opakovaném spuštění provisioningu.
4. Registrace proběhne na Zabbix serveru `enceladus.pfsense.cz`, kde jsou již připravena pravidla pro hosty s `HostMetadata=SPOS`. Po registraci ověřte, že má váš host přiřazenou skupinu, štítek a šablonu pro monitoring Linuxu agentem 2.
5. Ověřte, že se host v Zabbixu skutečně objevil a že z něj přicházejí data.

## Stručná nápověda k postupu

- Projděte si ukázku v adresáři [`Example/`](Example/). Je určena pro starší Ubuntu a starší Zabbix: použijte ji jen jako inspiraci pro strukturu Vagrantfile a skriptů.
- Na [stránce ke stažení Zabbixu](https://www.zabbix.com/download) vyberte větev **8.0 LTS**, distribuci **Debian 13** a komponentu **Agent 2**. Získané instalační kroky převeďte do skriptu. Po instalaci zkontrolujte verzi agenta a stav služby.
- V konfiguraci `/etc/zabbix/zabbix_agent2.conf` se zaměřte na parametry `Hostname`, `Server`, `ServerActive` a `HostMetadata`. Automatickou registraci zahajuje aktivní spojení agenta se serverem.
- Při hledání chyby ověřte dostupnost serveru `enceladus.pfsense.cz`, stav služby `zabbix-agent2` a její záznamy v systémovém žurnálu.

## Co odevzdat

Ve svém žákovském repozitáři doporučujeme odstranit vše, co jste ze šablony nevyužili, a ponechat jen soubory potřebné k odevzdání:

- Funkční `Vagrantfile` a vlastní instalační a konfigurační skripty, které automaticky připraví agenta.
- V adresáři `Images/` snímky obrazovky dokazující **registraci hosta s vaším jedinečným názvem** a **funkční monitorování** (například aktuálně přijímaná data v Zabbixu).
- Upravený `README.md` se stručným popisem postupu práce, způsobem spuštění a ověření výsledku. Můžete do něj přidat také vlastní tematický obrázek.

## Dokumentace

- [Zabbix 8.0: instalace z oficiálních balíčků](https://www.zabbix.com/documentation/8.0/en/manual/installation/install/packages)
- [Zabbix 8.0: automatická registrace aktivního agenta](https://www.zabbix.com/documentation/8.0/en/manual/discovery/auto_registration)
- [Zabbix 8.0: parametry konfigurace agenta 2](https://www.zabbix.com/documentation/8.0/en/manual/concepts/agent2/agent2_params)
