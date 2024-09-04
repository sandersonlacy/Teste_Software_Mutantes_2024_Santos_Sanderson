**Universidade Federal de Sergipe**

Teste de Software

Sanderson Lacy Alves dos Santos

Atividade 2

Testes de Mutação

São Cristóvão

2024

1. Projeto escolhido

Para execução da atividade foi escolhido o projeto “pomodorable” disponível em repositório no Github acessível através do endereço https://github.com/wmelvin/pomodorable.
O projeto se trata de uma implementação de um timer do tipo pomodoro, utilizado através de CLI (Command line interface). Todo o código foi escrito em linguagem Python, e no código fonte estão listados os casos de teste usados no projeto conforme requisitos para execução da atividade.

2. Preparação do ambiente

Antes da execução dos testes, obviamente foram instaladas as dependências do projeto e toolkits de testes de unidade, mutação e análise de cobertura. Para isso, foi usado o Pypi que é a ferramenta de gerenciamento de pacotes mais utilizada no Python.
Antes das instalações, foi criado o ambiente virtual:
Observação: Não foi instalado o venv pois já estava disponível na máquina utilizada para execução das atividades.
Criação e ativação do ambiente virtual:
```
python3 -m venv env
source env/bin/activate
```

Para instalação do pytest, pytest-cov e mutmut:
```
pip install pytest
pip install pytest-cov
pip install mutmut
```
Para instalação dos requisitos do pomodorable:
```
pip install -r requirements.txt
pip install pomodorable
```

Execução dos casos de teste

Dentro do diretório do projeto foi possível executar os testes de unidade definidos usando o comando:
```
pytest -vv
```
Como resultado, a maioria dos testes passaram, com exceção aos testes de ui que não foram executados por incompatibilidade com as bibliotecas instaladas.

Ao executar o comando
```
pytest -vv –cov –cov-branch –cov-report=html
```
Podemos gerar um documento html que especifica a nível de arquivo, função e classe, a cobertura dos nossos casos de teste

Ao clicar em um dos arquivos listados no índice, podemos ver detalhes da cobertura do arquivo em questão.

Fazendo o teste de mutação usando o comando
```
mutmut run –paths-to-mutate=/home/sanderson/Documentos/tsw_code/atividade2/env/lib/python3.10/site-packages/pomodorable
```
obtivemos os seguintes resultados:

Observamos na imagem acima que foram criados 1530 mutantes no código, sendo que destes, 453 foram eliminados, porém outros 1068 continuaram vivos, o que significa que a cobertura e qualidade dos testes criados não é ideal.

Usando o comando mutmut results obtivemos a lista de mutantes por id, e por resultado (Killed, suspicious, survived, skipped).

Foi usado o comando mutmut html para gerar um relatório para cada um dos arquivos que foram afetados pela mutação. Nesta atividade foi analisado exclusivamente o arquivo cli.py:

Observemos o relatório do mutante não eliminado de id 49:

O mutmut alterou o operador relacional de > para >=. Sendo assim, os testes criados não puderam identificar a alteração e o mutante continuou vivo. O código original nos diz que csv_date deve ser maior que end_date, porém não há caso de teste para csv_date e end_date sendo iguais. Podemos criar um teste unitário em tests/test_cli.py para suprir esta falta:

Criamos uma nova função chamada test_export_csv_for_date_range_mutant, e para ela, criamos duas possíveis execuções em que as datas definidas para os argumentos csv_date e end_date são iguais.

Após isso, criamos dois objetos date1 e date2 que representam os argumentos capturados. Abaixo, adicionamos um assert que garante que date1 e date2 são iguais e não devem retornar o resultado que seria disparado pelo mutante.

Com as alterações salvas, vamos executar os testes de mutação mais uma vez:

Observe que desta vez apenas 1062 mutantes continuaram vivos, ou seja, nossa alteração eliminou 6 mutantes.

Vamos gerar e verificar o relatório mutmut buscando o mutante 49:

Observe que ele não se encontra mais listado como survived. Portanto, nosso novo caso de teste proporcionou o efeito desejado.

4. Conclusão

Podemos observar que o pytest juntamente com o mutmut e o pytest-cov permite a escrita de testes robustos e eficazes. Os testes de mutação possibilitam ao testador identificar possíveis buracos nos casos de teste. Essa técnica aliada a uma análise de cobertura principalmente nos módulos “core” do projeto, aumenta consideravelmente a qualidade do software resultante.
