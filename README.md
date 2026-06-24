# Projeto Consulta e Atualização de Clientes em CICS

Projeto desenvolvido em COBOL no ambiente **TK5/MVS 3.8j**, utilizando **TN3270** e **KICKS** para simular ambiente CICS.

O sistema implementa uma aplicação online para consulta e atualização de clientes armazenados em um arquivo VSAM. A transação `CLIE` executa o programa `PROJETO7`, que interage com uma tela BMS e com o arquivo `CLIENTES`.

---

## Lógica de Funcionamento

Ao executar a transação `CLIE`, o sistema apresenta a tela de consulta de clientes.

O usuário informa o código do cliente e utiliza as teclas de função para consultar, alterar ou sair da transação.

* **PF5** consulta o cliente pelo código informado.
* **PF6** atualiza somente telefone e cidade.
* **PF3** encerra a transação e retorna ao terminal do KICKS.



---

## Em Funcionamento no TK5 com o KICKS com testes das funcionalidades

<img width="1800" height="1013" alt="2026-06-24 02-28-00" src="https://github.com/user-attachments/assets/e6a098fe-48c0-461a-8684-f991f368ced1" />

---

## Fluxograma PF5 - Consulta

```mermaid
flowchart TD
    A[Usuário informa o código] --> B[Pressiona PF5]
    B --> C{Código informado?}

    C -- Não --> D[Exibe: INFORME O CODIGO.]
    D --> Z[Mostra a tela novamente]

    C -- Sim --> E[READ no VSAM CLIENTES]
    E --> F{Cliente encontrado?}

    F -- Sim --> G[Move nome, telefone e cidade para a tela]
    G --> H[Exibe: CLIENTE ENCONTRADO.]
    H --> Z

    F -- Não --> I[Limpa nome, telefone e cidade]
    I --> J[Exibe: CLIENTE NAO ENCONTRADO.]
    J --> Z

    F -- Erro --> K[Exibe: ERRO NA CONSULTA.]
    K --> Z
```

---

## Fluxograma PF6 - Atualização

```mermaid
flowchart TD
    A[Usuário informa código, telefone e cidade] --> B[Pressiona PF6]
    B --> C{Código informado?}

    C -- Não --> D[Exibe: INFORME O CODIGO.]
    D --> Z[Mostra a tela novamente]

    C -- Sim --> E[READ UPDATE no VSAM CLIENTES]
    E --> F{Cliente encontrado?}

    F -- Não --> G[Exibe: CLIENTE NAO ENCONTRADO.]
    G --> Z

    F -- Erro --> H[Exibe: ERRO NA CONSULTA.]
    H --> Z

    F -- Sim --> I[Atualiza telefone e cidade no registro]
    I --> J[REWRITE no VSAM CLIENTES]
    J --> K{Gravação concluída?}

    K -- Sim --> L[Exibe: ALTERACAO REALIZADA.]
    L --> Z

    K -- Não --> M[Exibe: ERRO AO ATUALIZAR.]
    M --> Z
```

---

## Arquivos do Projeto

```text
PROJETO7.cbl        Programa COBOL executado pela transação CLIE
MAPSP7.bms          Fonte do mapa BMS da tela
VSAMP7.jcl          JCL para criar e carregar o arquivo VSAM CLIENTES
MAPP7.jcl           JCL para gerar o mapa BMS
BUILDP7.jcl         JCL para compilar e linkar o programa COBOL
```

---



## Tela do Sistema

<img width="1187" height="552" alt="image" src="https://github.com/user-attachments/assets/cb19db85-2ea4-4cf1-b4fc-9079b068ea0c" />

---

### Configuração de apoio usada no KICKS

Para a aplicação funcionar no ambiente KICKS, foram necessarias mais alguns arquivos configurados :

```text
PCT  -> A transação CLIE apontando para o programa PROJETO7
PPT  -> Programa PROJETO7 e mapset MAPSP7
FCT  -> Arquivo VSAM CLIENTES
```

Essas configurações foram usadas apenas para permitir a execução no KICKS.

---

## Comandos CICS/KICKS Utilizados

```text
SEND      Envia a tela BMS para o terminal
RECEIVE   Recebe os campos preenchidos pelo usuário
RETURN    Encerra a transação
READ      Consulta o cliente no VSAM
REWRITE   Atualiza o registro no VSAM
```

---

## Objetivo do Projeto

Este projeto tem como objetivo praticar desenvolvimento COBOL online em ambiente mainframe, utilizando transações CICS/KICKS, mapa BMS, interação com terminal 3270, consulta e atualização de registros VSAM. E a aplicação foi executada com sucesso no ambiente TK5/KICKS, permitindo consultar clientes e persistir alterações de telefone e cidade.
::: 
