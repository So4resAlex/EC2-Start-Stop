# EC2-Start-Stop

#### Este repositório foi baseado nas documentações abaixo. 

[EC2 Start Stop automatically using Lambda function](https://gist.github.com/amitavroy/26089061c7037e1ade77497504241618)  
[EC2 Start Stop automatically using Lambda function video](https://www.youtube.com/watch?v=Yq-z8yT7Aq0)

Para utilizar as funções Lambda de [Start](https://github.com/So4resAlex/EC2-Start-Stop/blob/main/lambda_EC2_Start.py) e [Stop](https://github.com/So4resAlex/EC2-Start-Stop/blob/main/lambda_EC2_Stop.py)
você deve criar uma Politica no IAM.

1.    No console do Lambda, escolha Criar função.

2.    Escolha Criar a partir do zero.

3.    Em Informações básicas, adicione o seguinte:  
Em Nome da função, insira um nome que a identifique como a função usada para interromper suas instâncias do EC2. Por exemplo, "StopEC2Instances".
Para Tempo de execução, escolha Python 3.9.
Em Permissões, expanda Alterar função de execução padrão.
Em Função de execução, selecione Usar uma função existente.
Em Função existente, escolha a função do IAM que você criou.

4.    Escolha Criar função.

5.    Em Código, Código-fonte, copie e cole o código a seguir no painel do editor de código (lambda_function). Esse código interrompe as instâncias do EC2 identificadas.

Exemplo de código de função: [interrompendo instâncias do EC2](https://github.com/So4resAlex/EC2-Start-Stop/blob/main/lambda_EC2_Stop.py)

```

import boto3
region = 'us-east-1' //Para a variavel region adicione a região que as suas instancia estão. 
instances = ['i-001e2d04e37ccde3b']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    print('Stopping instances')
    ec2.stop_instances(InstanceIds=instances)
    
```
Importante: para região, substitua "us-west-1" pela Região da AWS na qual estão suas instâncias. Para instâncias, substitua os IDs de instância do EC2 de exemplo pelos IDs das instâncias específicas que você deseja interromper e iniciar.

6.    Escolha Implantar.

7.    Na guia Configuração, escolha Configuração geral, Editar. Defina o Tempo limite para 10 segundos e selecione Salvar.

Observação: defina as configurações da função do Lambda conforme necessário para seu caso de uso. Por exemplo, se você quiser interromper e iniciar várias instâncias, precisará de valores diferentes no Tempo limite e Memória.

8.    Repita as etapas de 1 a 7 para criar outra função. Faça o seguinte de forma diferente para que essa função inicie suas instâncias do EC2:

Na etapa 3, insira um Nome de função diferente daquele usado antes. Por exemplo, “StartEC2Instances”.
Na etapa 5, copie e cole o seguinte código no painel do editor no editor de código (lambda_function):

Exemplo de código de função: [iniciando instâncias do EC2](https://github.com/So4resAlex/EC2-Start-Stop/blob/main/lambda_EC2_Start.py)

Teste suas funções do Lambda
1.    No console do Lambda, escolha Funções.

2.    Escolha uma das funções criadas.

3.    Selecione a guia Código.

4.    Na seção Código-fonte, selecione Testar.

5.    Na caixa de diálogo Configurar evento de teste, escolha Criar novo evento de teste.

6.    Insira um Nome de evento. Em seguida, escolha Criar.

Observação: não é necessário alterar o código JSON para o evento de teste; a função não o usa.

7.    Escolha Testar para executar a função.

8.    Repita as etapas de 1 a 6 para a outra função criada.

Dica: é possível verificar o status de suas instâncias do EC2 antes e depois do teste para confirmar se suas funções funcionam conforme o esperado.

Crie regras do EventBridge que acionam suas funções do Lambda
1.    Abra o console do EventBridge.

2.    Selecione Create rule (Criar regra).

3.    Insira um Nome para sua regra, como “StopEC2Instances”. Opcionalmente, você pode inserir uma Descrição.

4.    Em Definir padrão, selecione Programação.

5.    Execute um dos seguintes procedimentos:

Para Taxa fixa de, insira um intervalo de tempo em minutos, horas ou dias.
Para Expressão Cron, insira uma expressão que diga ao Lambda quando interromper suas instâncias. Para obter informações sobre a sintaxe da expressão, consulte Programar expressões para regras.
Nota: as expressões Cron seguem o formato UTC. Certifique-se de ajustar a expressão para seu fuso horário preferido.

6.    Em Selecionar destinos, escolha a função Lambda no menu suspenso Destino.

7.    Para Função, escolha a função que interrompe suas instâncias do EC2.

8.    Role a página e selecione Criar.

9.    Repita as etapas de 1 a 8 para criar uma regra que inicie as instâncias do EC2. Faça o seguinte de forma diferente:

Insira um nome para sua regra, como “StartEC2Instances”.
