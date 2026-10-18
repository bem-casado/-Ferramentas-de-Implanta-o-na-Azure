# Ferramentas de Implantação na Azure

Este repositório foi criado para o desafio **Ferramentas de Implantação na Azure**, da **DIO (Digital Innovation One)**. Ele reúne conceitos sobre as principais ferramentas para criar, configurar e administrar recursos do Microsoft Azure.

O objetivo é compreender como diferentes interfaces e técnicas de **automação e infraestrutura como código (IaC)** ajudam a realizar implantações reproduzíveis, seguras e organizadas.

> Este README apresenta um resumo de estudos e exemplos ilustrativos. Não afirma que os comandos foram executados ou que recursos foram provisionados durante o laboratório.

## Objetivos do desafio

- Identificar as principais ferramentas para implantação de recursos Azure.
- Comparar interfaces gráficas, linha de comando e infraestrutura como código.
- Entender o papel dos modelos ARM e da linguagem Bicep.
- Conhecer automação de implantações com pipelines.
- Aplicar boas práticas de organização, segurança e controle de custos.

## 1. Azure Portal

O **Azure Portal** é a interface web para gerenciar serviços da plataforma.

Por ele, é possível criar recursos, definir configurações, consultar métricas, visualizar custos e administrar permissões.

**Vantagens:** facilidade de uso e visualização dos recursos. **Limitações:** atividades manuais repetidas podem levar a divergências entre ambientes.

## 2. Azure Cloud Shell

O **Azure Cloud Shell** fornece um terminal acessível pelo navegador, com opções de **Bash** e **PowerShell** e ferramentas Azure pré-configuradas.

É útil para executar comandos sem preparar um ambiente local completo. Algumas funcionalidades e persistência dependem das configurações de armazenamento da sessão.

## 3. Azure CLI

A **Azure CLI** é uma ferramenta de linha de comando multiplataforma para gerenciar recursos Azure.

Exemplo de consulta:

```bash
az login
az account show
az group list --output table
```

Esses comandos autenticam uma sessão, mostram a assinatura selecionada e listam grupos de recursos. Antes de operações de escrita, é importante conferir a assinatura ativa.

## 4. Azure PowerShell

O módulo **Az PowerShell** permite administrar recursos usando comandos do PowerShell.

Exemplo de consulta:

```powershell
Connect-AzAccount
Get-AzContext
Get-AzResourceGroup
```

É especialmente útil em ambientes com scripts e rotinas operacionais baseados em PowerShell.

## 5. Azure Resource Manager (ARM)

O **Azure Resource Manager** é a camada de gerenciamento e implantação de recursos da Azure.

Os **ARM Templates** são arquivos declarativos em JSON que descrevem os recursos e suas configurações. Isso facilita versionamento e padronização entre ambientes.

Exemplo conceitual:

```text
Template ARM
├── Parâmetros
├── Definições de recursos
└── Saídas
        ↓
Azure Resource Manager
        ↓
Recursos da assinatura
```

## 6. Bicep

O **Bicep** é uma linguagem declarativa criada para definir infraestrutura Azure, oferecendo sintaxe mais concisa que templates ARM em JSON.

Exemplo ilustrativo de definição de Storage Account:

```bicep
param location string = resourceGroup().location
param storageName string

resource storage 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}
```

O nome da conta precisa ser globalmente único e atender às regras da Azure. Esse exemplo define o recurso; não cria nada até que uma implantação seja executada.

## 7. Terraform

O **Terraform** é uma ferramenta de infraestrutura como código que pode provisionar recursos em diferentes provedores, incluindo o Azure.

As configurações são escritas em HCL e normalmente passam por etapas de inicialização, planejamento e aplicação:

```text
Código Terraform
       ↓
terraform init
       ↓
terraform plan
       ↓
terraform apply
       ↓
Recursos gerenciados
```

O arquivo de estado deve ser protegido, pois pode conter informações sensíveis. Alterações devem ser revisadas antes da aplicação.

## 8. Azure DevOps e GitHub Actions

Pipelines automatizam processos como testes, validação de infraestrutura e implantação.

Exemplo:

```text
Alteração no Git
       ↓
Pull Request e revisão
       ↓
Validações / Testes
       ↓
Aprovação quando exigida
       ↓
Implantação controlada
       ↓
Verificação do resultado
```

É recomendável usar identidades com permissões mínimas e autenticação federada, quando disponível, evitando segredos permanentes em arquivos.

## 9. Comparação das ferramentas

| Ferramenta | Uso principal | Característica |
| --- | --- | --- |
| Azure Portal | Gerenciamento visual | Interface gráfica |
| Azure Cloud Shell | Administração pelo navegador | Terminal pronto para uso |
| Azure CLI | Automação e administração | Comandos multiplataforma |
| Azure PowerShell | Scripts administrativos | Integração com PowerShell |
| ARM Templates | Infraestrutura declarativa | JSON |
| Bicep | Infraestrutura declarativa Azure | Sintaxe simplificada |
| Terraform | IaC multiplataforma | HCL e gerenciamento de estado |
| GitHub Actions / Azure DevOps | Entrega automatizada | Pipelines e verificações |

## 10. Exemplo de implantação controlada

Para criar um ambiente de testes com uma aplicação e armazenamento:

```text
Repositório Git
   └── infra/
       ├── main.bicep
       └── parameters.dev.json
            ↓
Revisão de mudanças
            ↓
Validação e planejamento (what-if)
            ↓
Implantação autorizada
            ↓
Resource Group
       ├── Aplicação
       └── Storage Account
            ↓
Monitoramento e custos
```

A opção **what-if** do ARM/Bicep ajuda a visualizar mudanças previstas, mas seus resultados devem ser avaliados junto às verificações de permissões, dependências e políticas do ambiente.

## Boas práticas

1. Versionar definições de infraestrutura no Git.
2. Separar configurações de desenvolvimento e produção.
3. Utilizar parâmetros para evitar duplicações desnecessárias.
4. Nunca armazenar credenciais ou segredos no repositório.
5. Aplicar o princípio do menor privilégio.
6. Revisar o plano de mudanças antes de implantar.
7. Configurar auditoria e monitoramento.
8. Identificar recursos com tags e nomes padronizados.
9. Acompanhar custos e remover recursos de estudo não utilizados.
10. Documentar procedimentos de recuperação e reversão quando aplicável.

## Conclusão

As ferramentas de implantação do Azure atendem a diferentes necessidades. O **Portal** ajuda em tarefas visuais, enquanto **CLI** e **PowerShell** facilitam automações. **ARM, Bicep e Terraform** tornam configurações reproduzíveis, e **pipelines** permitem validar e controlar alterações.

O principal aprendizado é que uma implantação confiável deve combinar **automação, revisão, segurança e observabilidade**, escolhendo a ferramenta de acordo com a complexidade do projeto.

## Referências

- [Portal do Azure](https://portal.azure.com/)
- [Azure Cloud Shell](https://learn.microsoft.com/pt-br/azure/cloud-shell/overview)
- [Azure CLI](https://learn.microsoft.com/pt-br/cli/azure/)
- [Azure PowerShell](https://learn.microsoft.com/pt-br/powershell/azure/)
- [Azure Resource Manager](https://learn.microsoft.com/pt-br/azure/azure-resource-manager/management/overview)
- [Bicep](https://learn.microsoft.com/pt-br/azure/azure-resource-manager/bicep/overview)
- [Terraform no Azure](https://learn.microsoft.com/pt-br/azure/developer/terraform/overview)
- [GitHub Actions para Azure](https://learn.microsoft.com/pt-br/azure/developer/github/github-actions)
- [Azure DevOps](https://learn.microsoft.com/pt-br/azure/devops/)

---

**Desafio de Projeto — DIO**
