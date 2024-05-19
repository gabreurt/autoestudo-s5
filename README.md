# Autoestudo ponderado de Continuous Deploy com GitHub Actions e AWS

## TEMA 1 - A importância de CI/CD no desenvolvimento de software e como ele melhora a eficiência dos times de desenvolvimento
A Integração Contínua e a Entrega Contínua são práticas essenciais no desenvolvimento moderno de software, permitindo que as equipes de desenvolvimento entreguem software de alta qualidade de maneira mais rápida e eficiente. Elas automatizam o processo de integração de código, teste e implantação, reduzindo significativamente o tempo necessário para lançar novas funcionalidades e correções.
Uma das principais vantagens é a automação dos testes e das implantações. Cada alteração no código é automaticamente integrada e testada, garantindo que erros sejam detectados e corrigidos rapidamente. Isso reduz o risco de problemas críticos que podem surgir durante a integração manual de grandes volumes de código.
Além disso, a CI/CD promove um ciclo de desenvolvimento mais ágil, permitindo que as equipes liberem pequenas atualizações com maior frequência. Isso não apenas melhora a resposta às necessidades do mercado, mas também facilita a correção de bugs e a implementação de melhorias incrementais. Com a CI/CD, o feedback contínuo e as iterações rápidas tornam o processo de desenvolvimento mais eficiente e colaborativo.



## TEMA 2 - A estrutura e os componentes principais de um workflow do GitHub Actions
O GitHub Actions é uma poderosa ferramenta de automação de fluxo de trabalho que permite aos desenvolvedores automatizar, personalizar e executar seus fluxos de trabalho de desenvolvimento de software diretamente no repositório do GitHub. A estrutura de um workflow do GitHub Actions é definida em arquivos YAML que descrevem os eventos que disparam o workflow e as ações a serem executadas.

Os principais componentes de um workflow do GitHub Actions são:

Os eventos que iniciam o workflow, como push, pull request, ou eventos manuais. Por exemplo:
```
on:
  push:
    branches:
      - main
```

Os jobs, que são conjuntos de etapas que são executadas em um ambiente de execução específico. Cada job é executado em um runner que pode ser hospedado pelo GitHub ou auto-hospedado.
```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
```

As steps, que são as instruções individuais que compõem um job. Elas podem executar comandos, usar ações definidas pelo GitHub ou pela comunidade, ou usar ações personalizadas.
```
steps:
  - name: Setup Node.js
    uses: actions/setup-node@v2
    with:
      node-version: '14'
  - name: Install dependencies
    run: npm install
```

E as actions, que são tarefas reutilizáveis que podem ser combinadas para criar um fluxo de trabalho personalizado.

## TEMA 3 - A função e a importância do AWS CloudFormation na automação da infraestrutura, anexe o arquivo cloudformation que está no artigo e explique-o
O AWS CloudFormation é um serviço de infraestrutura como código que permite aos desenvolvedores e administradores de sistemas criar, gerenciar e provisionar recursos da AWS de maneira automatizada e previsível. Usando templates declarativos escritos em YAML ou JSON, ele orquestra a criação e configuração dos recursos da AWS, eliminando a necessidade de configuração manual e reduzindo a possibilidade de erros humanos.

O CloudFormation é importante principalmente por ter o papel de automatizar a implantação da infraestrutura visto que permite que a infraestrutura seja definida como código, facilitando a criação, atualização e exclusão de recursos de maneira consistente e reproduzível. Além disso, ele facilita a gestão da infraestrutura que agora pode ser versionada, compartilhada e reutilizada, permitindo práticas de DevOps mais eficientes. Em conclusão, ele ajuda a garantir que todos os recursos sejam configurados de acordo com as melhores práticas e políticas organizacionais.


