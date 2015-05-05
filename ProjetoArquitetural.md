### Arquitetura ###

**Descrição**

Documento completo [aqui.](http://schemagenerator2.googlecode.com/files/relatorioArquitetural.pdf)

http://schemagenerator2.googlecode.com/files/architecture.JPG


A entrada da ferramenta, esquema base, é um banco de dados na sintaxe do Sistema de Gerenciamento de Banco de Dados Oracle 10g Express Edition, estes serão lidos para a geração de novos esquemas, os sub esquemas.

O módulo COMUNICATION é encarregado de realizar a comunicação com o SGBD para ler o esquema base e guardá-lo em uma estrutura de dados para poder ser utilizado posteriormente por outros módulos do programa. A comunicação entre o SGBD Oracle e a ferramenta é realizada via JDBC.

O módulo SCHEMA DRAWER recebe do módulo COMUNICATION o esquema base e através das bibliotecas GWT e Swing de Java, envia para a INTERFACE um conjunto de elementos (grafos) que são exibidos para o usuário. Nesta representação serão mostradas as tabelas do esquema base e seus relacionamentos.

O módulo SCHEMA EXTRACTOR é responsável pela geração de sub esquemas, este recebe o esquema base do COMUNICATION e acessa  CONTROLLER para aplicar as técnicas, escolhidas pelo usuário, nos sub esquemas gerados. Serão implementadas em Projeto 1 três técnicas: adição e remoção de colunas de tabelas e mudança de label.

A mudança de label se caracteriza pela substituição de termos presentes no esquema por sinônimos, como por exemplo, uma tabela Professor se transforma em uma tabela Docente, uma vez que no domínio utilizado pelo cliente os termos são sinônimos. Para a mudança de label será utilizado um dicionário de sinônimos externo através da api do dicionário [WordNet](http://wordnet.princeton.edu/). Todas as modificações serão registradas em um LOG para controle do usuário e dos desenvolvedores. As técnicas estão nos módulos MODIFIER que são acessados pelo CONTROLLER.

O módulo CONTROLLER é a principal comunicação entre os módulos. Ele controla a aplicação das técnicas e envia os sub esquemas gerados e modificados para o módulo CODER.

O CODER prepara a saída dos sub esquemas gerados, estas podem ser em scripts OWL e/ou scripts SQL de acordo com a solicitação do usuário do programa. Para cada OWL solicitada é criado um arquivo “.owl”  e para cada SQL são criados arquivos “.txt”, um contendo os Creates, para criar as tabelas do sub-esquema, e um contendo os Alters para adicionar constraints nas tabelas criadas.

Os arquivos de saída no formato OWL serão entradas para o Ontology Editor (usaremos a ferramenta [Protegé](http://protege.stanford.edu/)). Este ambiente é responsável por leitura de arquivos OWL. O Ontology Editor testará se o script produzido não contém erro sintático, se o script for lido sem erros, a geração dos scripts foi correta.

E os arquivos de saída no formato SQL serão entradas para o Database Tool (usaremos a ferramenta [ISQL Plus](http://www.oracle.com/technology/docs/tech/sql_plus/index.html)) . Este ambiente é responsável por leitura de arquivos SQL. O ISQL Plus testará se o script produzido não contém erro sintático, se o script for lido sem erros, a geração dos scripts foi correta.


A INTERFACE está encarregada de conter janelas e botões para iteração com o usuário, bem como a representação gráfica gerada pelo módulo SCHEMA DRAWER. É através da INTERFACE que o usuário especifica onde está localizado (pode ser em outra máquina ligada na rede, basta digitar o ip dessa máquina) o SGBD Oracle que hospeda o esquema base, juntamente com o nome do usuário e senha necessária para acessar esse SGBD. Utilizando a INTERFACE o usuário determina quantos sub esquemas serão gerados, quantas tabelas em cada sub-esquema, o tipo de saída dos scripts (OWL e/ou SQL), escolhe se será utilizado o dicionário de sinônimos para mudança de label e se será utilizado técnicas de adição/remoção de colunas nas tabelas, e por fim escolhe o diretório para saída dos scripts gerados.