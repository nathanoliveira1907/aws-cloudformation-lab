# 🚀 vSBC Infrastructure-as-Code (AWS CloudFormation)

Template de infraestrutura para o deploy automatizado de instâncias **vSBC (SBC virtual)** na AWS. O lab utiliza instâncias da família **C5**, normalmente recomendadas para aplicações VoIP.

## 📋 Pré-requisitos

Antes de iniciar, você precisará de:

1. Uma conta ativa na **AWS**.

2. **AWS CLI** instalado em sua máquina ([Guia de instalação](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)).
É possível seguir os passos:
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
Verifique com: ```aws --version```

3. Ter o **VS Code** instalado na máquina com as seguintes extensões:
- AWS Toolkit
- CloudFormation Linter
- YAML (da Red Hat)

4. Instalar CFN-Lint, ele é uma ferramenta que efetua análises em templates de CloudFormation:

```pip install cfn-lint``

Pode ser validado rodando o comando cfn-lint e passando o template (em yaml o json) como argumento:

```cfn-lint ec2x2.yaml```

3. Permissões de IAM para criar recursos de EC2, EIP e CloudFormation.


## 🔑 1. Preparação das Chaves e Acesso

### Criar Chaves de Acesso (IAM)
1. No console AWS, vá em **IAM** > **Users**.
2. Selecione seu usuário e vá na aba **Security credentials**.
3. Clique em **Create access key** e selecione **Command Line Interface (CLI)**.
4. **IMPORTANTE:** Baixe o arquivo `.csv` ou copie a `Access Key ID` e a `Secret Access Key`.

### Configurar o AWS CLI localmente
No seu terminal, execute:
```bash
aws configure

*Insira então as chaves de AK e SK geradas na nuvem*


🚀 2. Deploy da Infraestrutura
O deploy é realizado através do serviço CloudFormation, que orquestra a criação das instâncias e a associação dos IPs fixos.

Comando de Execução
Navegue até a pasta do projeto e execute:

Bash
aws cloudformation create-stack \
  --stack-name Stack-vSBC-VoIP \
  --template-body file://template.yaml \
  --region us-east-1
O que acontece durante o processo?

Validação: A AWS verifica a sintaxe do arquivo template.yaml.
Criação de EC2: Duas instâncias c5.large são iniciadas simultaneamente na VPC informada.
Reserva de EIP: Dois Elastic IPs (IPs fixos) são reservados.
Associação: O CloudFormation vincula cada IP à sua respectiva instância assim que elas atingem o estado running.

