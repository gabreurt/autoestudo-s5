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


O arquivo do artigo (no meu grupo usamos terraform) cria uma infraestrutura CI/CD usando GitHub Actions e CodeDeploy. 
O template cria uma infraestrutura robusta na AWS, incluindo redes, instâncias EC2, balanceamento de carga, grupos de Auto Scaling e integração com GitHub Actions para automatizar o processo de CI/CD.

A seção Metadata define a interface do CloudFormation, organizando os parâmetros em grupos para facilitar a configuração.

**ParameterGroups:**
  1. VPC Configurations: Parâmetros relacionados à configuração da VPC, incluindo intervalos de CIDR para sub-redes públicas e privadas.
  2. Autoscaling configurations: Parâmetros para configurar o Auto Scaling, como ID da imagem, tipo de instância e tamanhos mínimo, máximo e desejado do grupo de Auto Scaling.
  3. Github configurations: Parâmetros para configurar o repositório do GitHub e a lista de impressões digitais para autenticação do OIDC.

### Parâmetros
  1. **VpcCIDR:** Intervalo de IP para a VPC.
  2. **PublicSubnet1CIDR e PublicSubnet2CIDR:** Intervalos de IP para sub-redes públicas nas zonas de disponibilidade 1 e 2.
  3. **PrivateSubnet1CIDR e PrivateSubnet2CIDR:** Intervalos de IP para sub-redes privadas nas zonas de disponibilidade 1 e 2.
  4. **ImageId:** ID da imagem AMI da instância EC2.
  5. **InstanceType:** Tipo da instância EC2 a ser usada.
  6. **AutoScalingGroupMinSize, AutoScalingGroupMaxSize, AutoScalingGroupDesiredCapacity:** Configurações de dimensionamento automático.
  7. **ThumbprintList:** Impressões digitais do certificado OIDC.
  8. **GithubRepoName:** Nome do repositório GitHub.

### Recursos
1. **VPC:** Cria uma VPC com suporte DNS habilitado.

2. **InternetGateway e InternetGatewayAttachment:** Cria e anexa um gateway de internet à VPC.

3. **Subnets:** Cria sub-redes públicas e privadas em diferentes zonas de disponibilidade.

4. **NatGateway1 e NatGateway1EIP:** Cria um NAT Gateway com um endereço IP elástico para permitir que instâncias em sub-redes privadas acessem a internet.

5. **RouteTables e Routes:** Configura tabelas de rotas e associações para sub-redes públicas e privadas.

6. **SecurityGroups:** <BR>
  **6.1 ALBSecurityGroup:** Grupo de segurança para o Application Load Balancer (ALB), permitindo tráfego na porta 8080. <br>
  **6.2 WebappSecurityGroup:** Grupo de segurança para a aplicação web, permitindo tráfego do ALB.

7. **IAM Roles e Policies:** <BR>
  **7.1 WebappRole:** Função IAM para a aplicação web, permitindo que instâncias EC2 e o CodeDeploy assumam a função. <br>
  **7.2 IDCProvider:** Provedor OIDC para autenticação com GitHub Actions. <br>
  **7.3 GitHubIAMRole:** Função IAM para permitir que GitHub Actions interaja com o CodeDeploy.

8. **CodeDeploy Application e DeploymentGroup:** Configura o CodeDeploy para implantar a aplicação em um grupo de Auto Scaling.

9. **AutoScaling Group e Launch Configuration:** Define um grupo de Auto Scaling e uma configuração de lançamento para instâncias EC2, incluindo a instalação do Tomcat e do agente do CodeDeploy.

10. **S3 Bucket:** Cria um bucket S3 para armazenar pacotes de implantação.

11. **Load Balancer e Target Group:** 
  **11.1 ApplicationLoadBalancer:** Cria um ALB público.
  **11.2 ALBTargetGroup:** Grupo de destino para o ALB, definindo verificações de integridade.

### Outputs
1. **WebappUrl:** URL da aplicação web.
2. **DeploymentGroup:** Nome do grupo de implantação.
3. **DeploymentBucket:** Nome do bucket S3 de implantação.
4. **ApplicationName:** Nome da aplicação CodeDeploy.
5. **GithubIAMRoleArn:** ARN da função IAM para GitHub.


## TEMA 4 - Discuta como a integração de GitHub Actions com AWS CloudFormation e Amazon EC2 pode ser aplicada em projetos reais. Quais desafios você encontrou no seu projeto e como os solucionou?

A integração do GitHub Actions com AWS CloudFormation e Amazon EC2 pode ser aplicada de maneira prática em projetos reais para automatizar completamente o ciclo de vida de desenvolvimento e implantação. Em um cenário típico, um desenvolvedor faz commit do código no repositório GitHub, o GitHub Actions dispara um workflow que inclui a criação ou atualização da infraestrutura necessária usando CloudFormation e, em seguida, implanta a aplicação nas instâncias EC2. No meu projeto, estamos utilizando terraform, então serei generalista para falar sobre desafios e soluções.

Em projetos reais, O uso do AWS CloudFormation com GitHub Actions permite que equipes provisionem infraestrutura sob demanda, garantindo que os ambientes de desenvolvimento, teste e produção sejam consistentes e facilmente replicáveis. Também, integrar o GitHub Actions com o EC2 através do AWS CodeDeploy permite que o código seja automaticamente implantado em instâncias EC2 após cada commit ou merge, reduzindo o tempo de inatividade e acelerando a entrega de novas funcionalidades. E, utilizando grupos de autoescalonamento do EC2, as aplicações podem escalar automaticamente com base na demanda e se recuperar de falhas, melhorando a resiliência e disponibilidade dos serviços.

### Desafios e Soluções

#### Desafios:

1. **Configuração de Segurança:** Gerenciar segredos e credenciais de forma segura foi um dos principais desafios. Garantir que os segredos da AWS não fossem expostos foi crucial.

2. **Erros de Configuração:** Erros na configuração dos templates CloudFormation podem levar a falhas na criação de recursos, resultando em atrasos.

3. **Integração Complexa:** A integração inicial entre GitHub Actions, AWS CloudFormation e CodeDeploy exigiu um entendimento detalhado de cada serviço e como eles interagem.

#### Soluções:

1. **Uso de OIDC e IAM Roles:** Implementamos o provedor de identidade OIDC do IAM para permitir que o GitHub Actions interaja com a AWS sem a necessidade de armazenar credenciais longas, aumentando a segurança.
2. **Validação de Templates:** Utilizamos ferramentas de validação e linting para garantir que os templates CloudFormation fossem corretos antes de aplicá-los, reduzindo erros de configuração.
3. **Documentação e Scripts de Automação:** Criamos scripts e documentação detalhada para automatizar e documentar cada etapa do processo de integração, facilitando a configuração e a resolução de problemas.
