# RELATÓRIO DE IMPLEMENTAÇÃO DE SERVIÇOS AWS

**Data:** 25/10/2026

**Empresa:** Rede Pulso Farma (empresa fictícia, cenário de estudo)

**Responsável:** Moisés Santos de Oliveira

## Introdução

Este relatório mostra como cortar custos de nuvem na Rede Pulso Farma, uma rede de farmácias imaginada para este estudo, usando 3 serviços AWS. O objetivo é economia imediata: sem migrar sistema e sem mexer na rotina das lojas.

Escolhi pensar na conta da AWS como um paciente. Primeiro o **exame**, para ver onde tem excesso. Depois o **prazo de validade**, para tirar o que já venceu. Por fim o **termômetro**, para saber na hora quando a conta "tiver febre".

**Cenário imaginado:** uma rede com várias lojas, sistema de vendas e estoque rodando em servidores EC2 e backups guardados em disco. Não tenho os números reais de custo dessa rede, então não prometo percentual de economia. O foco é mostrar onde olhar e como medir.

## Descrição do Projeto

O projeto de implementação de ferramentas foi dividido em 3 etapas, cada uma com seus objetivos específicos. A seguir, serão descritas as etapas do projeto:

Etapa 1 – O exame:
- **AWS Compute Optimizer**
- **Foco da ferramenta:** achar servidores maiores do que o necessário.
- **Descrição de caso de uso:** ele olha o uso real de CPU e memória das instâncias EC2 e diz quando um servidor está sobrando. Em farmácia isso é comum: o sistema foi dimensionado para o dia de maior movimento e fica assim o ano todo. A recomendação já vem com a economia estimada. No uso básico, o serviço não tem custo adicional.
- **Cuidado:** testar a troca primeiro em ambiente de teste e fazer fora do horário de pico. O caixa da loja não pode travar.

Etapa 2 – O prazo de validade:
- **Amazon Data Lifecycle Manager (Amazon EBS)**
- **Foco da ferramenta:** apagar sozinho os backups (snapshots) que já venceram.
- **Descrição de caso de uso:** snapshot é como remédio no armário: serve dentro do prazo e depois só ocupa espaço. Cada um continua sendo cobrado todo mês até alguém apagar, e quase ninguém lembra. Com uma política do Data Lifecycle Manager, a empresa define quantos backups guardar (por exemplo, os últimos 7 diários e 4 semanais) e o resto some. Dá para somar a troca de volumes gp2 por gp3, que custa até cerca de 20% menos por GB, sem trocar o servidor. O serviço em si não tem custo adicional.
- **Cuidado:** dado de saúde é dado sensível pela LGPD. Antes de ligar a política, combinar com o jurídico quanto tempo cada tipo de backup precisa ser guardado.
- **Caso real:** na minha conta de testes achei 2 snapshots de 30 GiB criados em março, ainda lá meses depois, cobrando sem uso. Foi isso que me levou a incluir esta etapa.

Etapa 3 – O termômetro:
- **AWS Cost Anomaly Detection (com AWS Budgets)**
- **Foco da ferramenta:** avisar quando o gasto sair do normal.
- **Descrição de caso de uso:** ele aprende o padrão de gasto de cada serviço e manda alerta por e-mail quando algo foge, como um ambiente de teste esquecido ligado no fim de semana. O AWS Budgets entra com um limite mensal e avisos em 80% e 100% do orçamento. Assim a economia das etapas 1 e 2 não escapa sem ninguém ver. O Cost Anomaly Detection é gratuito.
- **Cuidado:** definir quem recebe o alerta e quem age. Alerta sem dono vira só barulho.

## Conclusão

A implementação de ferramentas na Rede Pulso Farma tem como esperado **pagar só pelo servidor que usa (Compute Optimizer), parar de pagar por backup vencido (Data Lifecycle Manager) e descobrir desperdício novo no mesmo dia (Cost Anomaly Detection)**, o que aumentará a eficiência e a produtividade da empresa. Recomenda-se a continuidade da utilização das ferramentas implementadas e a busca por novas tecnologias que possam melhorar ainda mais os processos da empresa.

**Plano de 30 dias**
- Semana 1: ligar o Compute Optimizer e ler as recomendações.
- Semana 2: criar a política de backups, depois de combinar os prazos com o jurídico.
- Semana 3: ligar o Cost Anomaly Detection e o Budgets, com um responsável pelos alertas.
- Semana 4: comparar a conta do mês com a do mês anterior.

**Como saber se deu certo:** olhar 3 números, o custo mensal, a quantidade de servidores reduzidos e a quantidade de snapshots removidos.

**Quem ganha com isso**
- **Financeiro:** conta menor e sem surpresa no fim do mês.
- **TI:** menos máquina sobrando e alerta antes do problema crescer.
- **Loja:** o sistema continua rápido no horário de pico.
- **Jurídico:** backup com regra definida, e não apagado ou guardado por acaso.

Economizar na nuvem começa por parar de pagar o que ninguém usa.

## Anexos

- [Calculadora de preços da AWS](https://calculator.aws/): para estimar o custo antes de implementar.
- [AWS Compute Optimizer](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html)
- [Amazon Data Lifecycle Manager](https://docs.aws.amazon.com/ebs/latest/userguide/snapshot-lifecycle.html)
- [AWS Cost Anomaly Detection](https://docs.aws.amazon.com/cost-management/latest/userguide/manage-ad.html)

Assinatura do Responsável pelo Projeto:

Moisés Santos de Oliveira
