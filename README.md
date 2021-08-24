[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE.md) 
[![CLA](https://img.shields.io/badge/CLA%3F-Required-blue.svg)](https://mycroft.ai/cla) 
[![Team](https://img.shields.io/badge/Team-Mycroft_Core-violetblue.svg)](https://github.com/MycroftAI/contributors/blob/master/team/Mycroft%20Core.md) 
![Status](https://img.shields.io/badge/-Production_ready-green.svg)

![Unit Tests](https://github.com/mycroftai/mycroft-core/workflows/Unit%20Tests/badge.svg)
[![codecov](https://codecov.io/gh/MycroftAI/mycroft-core/branch/dev/graph/badge.svg?token=zQzRlkXxAr)](https://codecov.io/gh/MycroftAI/mycroft-core)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Join chat](https://img.shields.io/badge/Mattermost-join_chat-brightgreen.svg)](https://chat.mycroft.ai)

# EVA Assistente Virtual

Eva é uma assistente virtual por comandos de voz em lingua portuguesa, no caso com sotaque brasileiro
e de código aberto. Eva é baseada no assistente virtual Mycroft (em língua inglesa), procurando manter a compatibilidade com este último.
Detalhes sobre o Projeto EVA podem ser vistos no portal www.eva.fmrp.usp.br, dessenvolvido pela equipe da professora Drª Gleici Perdoná - www.gleici.fmrp.usp.br  e a sua equipe de desenvolvedores. Tiago Chiaveri e Rafael Fázio
empresa desenvolvedora: www.alicehuman.com

## Índice

- [Introdução](#introdução)
- [Executando EVA](#executando-eva)
- [Usando EVA](#usando-eva)
   * [Habilidades](#habilidades)
- [Rodando no Plano de Fundo](#rodando-no-plano-de-fundo)
   * [Configuração](#configuração)
   * [Usando EVA somente local](#usando-eva-somente-local)
   * [Chaves API de serviços](#api-key-services)
   * [Usando EVA por meio de um proxy](#[usando-eva-por-meio-de-um-proxy)
- [Colabora](#colaborar)
- [Links](#links)

## Introdução

Primeiro, baixe o código em seu sistema! O método mais simples é via git.
Digite no terminal:
- `cd ~ /`
- `git clone https://github.com/EVA-FMRP/eva-core.git`
- `cd eva-core`
- `bash dev_setup.sh`


Este script configura dependências e um [virtualenv] [about-virtualenv]. Se estiver executando em um ambiente além do Ubuntu / Debian, Arch ou Fedora, você pode precisar instalar manualmente os pacotes conforme instruído por dev_setup.sh.

[about-virtualenv]: https: //virtualenv.pypa.io/en/stable/

NOTA: O modo padrão para este repositório é 'dev', que deve ser considerado um trabalho em andamento. Se você quiser clonar uma versão mais estável, mude para o branch 'master'.

## Executando EVA

EVA fornece `start-mycroft.sh` para realizar tarefas comuns. Este script usa um virtualenv criado por `dev_setup.sh`. Supondo que você instalou mycroft-core em seu diretório inicial, execute:

- `cd ~/eva-core`
- `./start-mycroft.sh debug`

O comando "debug" iniciará os serviços de segundo plano (ouvinte de microfone, habilidade, messagebus e subsistemas de áudio), bem como exibirá uma Interface de Linha de Comando (CLI) baseada em texto que você pode usar para interagir com Mycroft e ver o conteúdo do vários registros. Alternativamente, você pode executar `./start-mycroft.sh all` para iniciar os serviços sem a interface de linha de comando. Posteriormente, você pode ativar a CLI usando `./start-mycroft.sh cli`.

Os serviços de segundo plano podem ser interrompidos como um grupo com:
- `./stop-mycroft.sh`

## Usando EVA

### Habilidades

Eva não é nada sem habilidades. Existem várias habilidades padrão que são baixadas automaticamente para o diretório `/opt/mycroft/skills`, mas a maioria precisa ser instalada explicitamente. Veja o [Skill Repo] (https://github.com/EVA-FMRP/habilidades#SejaBemVindo) para descobrir funcionalidades feitas por outros colaboradores. Por favor, compartilhe seu próprio trabalho!

## Rodando no Plano de Fundo

### Configuração
A configuração do Mycroft consiste em 4 localizações possíveis:
- `eva-core/mycroft/configuration/mycroft.conf` (padrão)
- [EVA Home] (https://home.mycroft.ai) (Remoto)
- `/etc/mycroft/mycroft.conf` (Sistema Operacional)
- `$XDG_CONFIG_DIR/mycroft/mycroft.conf` (que é por padrão` $HOME/.config/mycroft/mycroft.conf`) (USUÁRIO)

Quando o carregador de configuração é iniciado, ele procura nesses locais nesta ordem e carrega TODAS as configurações. As chaves que existem em vários arquivos de configuração serão substituídas pelo último arquivo que contém o valor. Este processo resulta em uma quantidade mínima sendo escrita para um dispositivo e usuário específicos, sem modificar os arquivos de distribuição padrão.

### Usando EVA somente local

Se você não deseja usar o serviço Mycroft Home, antes de iniciar o Mycroft pela primeira vez, crie `$HOME/.config/mycroft/mycroft.conf` com o seguinte conteúdo:

`` `
{
  "skills": {
    "blacklisted_skills": [
      "mycroft-configuration.mycroftai",
      "mycroft-pairing.mycroftai"
    ]
  }
}
`` `

### Serviços principais de API

O back-end EVA fornece acesso a uma variedade de chaves de API para serviços específicos. Sem emparelhar com o backend EVA-Mycroft, você precisará adicionar suas próprias chaves de API, instalar uma habilidade ou plug-in diferente para executar essa função ou não terá acesso a essa funcionalidade.

Estas são as chaves usadas atualmente no Mycroft Core por meio do backend Mycroft:

- [STT API, Google STT, Google Cloud Speech] (http://www.chromium.org/developers/how-tos/api-keys)
  - [Uma gama de serviços STT] (https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/stt-engine) estão disponíveis para uso com Mycroft.
- [API Weather Skill, OpenWeatherMap] (http://openweathermap.org/api)
- [Wolfram-Alpha Skill] (http://products.wolframalpha.com/api/)


### Usando EVA por meio de um proxy

Muitas escolas, universidades e locais de trabalho executam um `proxy` em sua rede. Se você precisa digitar um nome de usuário e senha para acessar a Internet externa, provavelmente você está atrás de um `proxy`.

Se você planeja usar o Mycroft por trás de um proxy, precisará executar uma etapa de configuração adicional.

_NOTA: Para completar este passo, você precisará saber o `hostname` e a` porta` do servidor proxy. Seu administrador de rede poderá fornecer esses detalhes

## Colaborar

Este é um projeto open source. Nós adoraríamos sua ajuda. Preparamos um guia de contribuição para ajudá-lo a começar.
Envolver-se

Este é um projeto open source. Esteja a vontade para modificar e colaborar com o projeto.


 ## Links
* [Creating a Skill](https://mycroft-ai.gitbook.io/docs/skill-development/your-first-skill)
* [Documentation](https://docs.mycroft.ai)
* [Skill Writer API Docs](https://mycroft-core.readthedocs.io/en/master/)
* [Release Notes](https://github.com/MycroftAI/mycroft-core/releases)
* [Mycroft Chat](https://chat.mycroft.ai)
* [Mycroft Forum](https://community.mycroft.ai)
* [Mycroft Blog](https://mycroft.ai/blog)

### Removendo a EVA
rm -rf ~/.mycroft
sudo rm -rf /opt/mycroft
rm -rf mycroft-core
sudo rm -rf /etc/mycroft/mycroft.conf

