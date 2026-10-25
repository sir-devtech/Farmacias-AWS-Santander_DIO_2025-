# RELATÓRIO DE IMPLEMENTAÇÃO DE SERVIÇOS AWS

**Data:** 25/10/2026

**Empresa:** Abstergo Industries

**Responsável:** Moisés Santos de Oliveira

## Introdução

Este relatório apresenta o processo de implementação de ferramentas na empresa Abstergo Industries, realizado por Moisés Santos de Oliveira. O objetivo do projeto foi elencar 3 serviços AWS, com a finalidade de realizar diminuição de custos imediatos.

A conta AWS foi tratada como um paciente: antes de comprar qualquer coisa nova, é preciso fazer o **exame** (descobrir onde há excesso), cuidar do **prazo de validade** (remover o que já venceu) e instalar um **termômetro** (para saber na hora quando a febre de custo aparece). Por isso os três serviços escolhidos não pedem migração nem reescrita de sistema, e dois deles não têm custo adicional pelo uso básico.

## Descrição do Projeto

O projeto de implementação de ferramentas foi dividido em 3 etapas, cada uma com seus objetivos específicos. A seguir, serão descritas as etapas do projeto:

Etapa 1 – O exame:
- **AWS Compute Optimizer**
- **Foco da ferramenta:** descobrir quais servidores estão maiores do que o necessário.
- **Descrição de caso de uso:** o Compute Optimizer analisa o uso real de CPU e memória das instâncias EC2 e recomenda um tamanho menor quando o servidor passa a maior parte do tempo ocioso. Em uma operação farmacêutica, é comum o sistema de retaguarda ter sido dimensionado para o pior dia do ano e continuar assim o ano inteiro. A recomendação sai pronta, com a economia estimada, e a troca de tamanho pode ser feita no mesmo dia. No uso básico, o serviço não tem custo adicional.

Etapa 2 – O prazo de validade:
- **Amazon Data Lifecycle Manager (Amazon EBS)**
- **Foco da ferramenta:** apagar automaticamente backups (snapshots) que já passaram da validade.
- **Descrição de caso de uso:** snapshots de disco são como medicamentos no armário: úteis enquanto estão dentro do prazo, e só ocupam espaço depois. Cada um continua sendo cobrado todo mês até que alguém o apague, e quase ninguém lembra de fazer isso. Com uma política do Data Lifecycle Manager, a empresa define quantos backups guardar (por exemplo, os últimos 7 diários e 4 semanais) e o restante é removido sozinho. Como complemento, volumes gp2 podem ser migrados para gp3, que custa até cerca de 20% menos por GB, sem trocar o servidor. O serviço em si não tem custo adicional.
- **Observação de laboratório:** na minha própria conta de testes encontrei 2 snapshots de 30 GiB criados em março que continuavam lá meses depois, sem uso e gerando cobrança. Foi esse caso real que me levou a incluir esta etapa.

Etapa 3 – O termômetro:
- **AWS Cost Anomaly Detection (com AWS Budgets)**
- **Foco da ferramenta:** avisar assim que o gasto sair do padrão.
- **Descrição de caso de uso:** o serviço aprende o padrão normal de gasto de cada serviço e manda um alerta por e-mail quando algo foge do esperado, como um ambiente de teste esquecido ligado no fim de semana ou um erro de configuração gerando tráfego. O AWS Budgets complementa com um limite mensal e avisos em 80% e 100% do orçamento. Assim, a economia das etapas anteriores não se perde sem ninguém perceber. O Cost Anomaly Detection é gratuito.

## Conclusão

A implementação de ferramentas na empresa Abstergo Industries tem como esperado **reduzir o gasto com servidores superdimensionados (Compute Optimizer), parar de pagar por backups vencidos (Data Lifecycle Manager) e evitar que desperdícios novos passem despercebidos (Cost Anomaly Detection)**, o que aumentará a eficiência e a produtividade da empresa. Recomenda-se a continuidade da utilização das ferramentas implementadas e a busca por novas tecnologias que possam melhorar ainda mais os processos da empresa.

Ordem recomendada: começar pela Etapa 1, porque ela mostra quanto há para economizar; seguir para a Etapa 2, que gera economia imediata e recorrente; e fechar com a Etapa 3, que protege o resultado. Em uma segunda fase, armazenamento de documentos no Amazon S3 com ciclo de vida e instâncias com processador Graviton podem ampliar a economia, mas exigem planejamento maior.

## Anexos

- [Calculadora de preços da AWS](https://calculator.aws/): para estimar o custo antes de implementar.
- [AWS Compute Optimizer](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html)
- [Amazon Data Lifecycle Manager](https://docs.aws.amazon.com/ebs/latest/userguide/snapshot-lifecycle.html)
- [AWS Cost Anomaly Detection](https://docs.aws.amazon.com/cost-management/latest/userguide/manage-ad.html)

Assinatura do Responsável pelo Projeto:

Moisés Santos de Oliveira
