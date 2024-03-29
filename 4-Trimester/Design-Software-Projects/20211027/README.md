# Aula 4 - 20211027
## Atividade 3
- Identificar os candidatos a Agregado.
    - Identificar candidatos a Evento de Domínio
        - Modelá-los em classes e apresentá-los no diagrama desse módulo.
    - Mostrar em diagramas de sequência a publicação e o tratamento desses eventos por clientes interessados

- Revisar os módulos para conterem os Agregados.

## Noções Básicas do Projeto Tático
- O ciclo de vida de um objeto do domínio
    - Alguns objetos são **transientes**: criados para serem usados em alguma computação e depois abandonados para serem excluídos da memória.
    - Outros são **persistentes**: devem **permanecer** após seu uso **na memória ativa**, tendo interpendência nem sempre trivial com outros objetos. Suas mudanças de estados devem ser consistentes, i.e., não se persiste objetos com estado inválido.
    - Desafios colocados pelos objetos persistentes:
        - Como manter a integridade de uma rede de objetos interdependentes ao longo de seu ciclo de vida?
        - Como evitar que o modelo se atole na complexidade da evolução do ciclo de vida dessa rede de objetos?

- **Agregado**: Encapsula a estrutura interna de objetos com sua **rede de associações**, mantendo a integridade dessa estrutura.
    - Quando persistir o Agregado ele estará integro

- Agregado é tão importante que existe essa frase "Projete para Agregados" - Ideia básica para DDD.

- **Repositório**: Fornece meios para armazenar, encontrar e recuperar Agregados, enquanto encapsula os mecanismos da infraestrutura envolvida.

- **Fábrica**: Cria e reconstitui objetos não triviais de modo íntegro.

- Retomando o Princípio LSP: **invariantes**
    - Regra das propriedades do princípio (LSP) trata dos invariantes da classe: propriedades globais das instâncias que devem ser preservadas ao longo de seus ciclos de vida (para todas as instâncias).
    - Os invariantes de uma classe expressam as regras de integridade da classe

### Agregado
- **Definição**: um aglomerado de objetos associados que devem ser tratados como uma unidade para fins de mudanças = um todo conceitual. Ele tem uma raiz, uma fronteira e invariantes.
    - A **raiz** – uma Entidade - é o **único membro do Agregado com o qual objetos externos podem manter referências**.Sugere-se que o nome da raiz seja o do Agregado, pois ela expressa o todo conceitual modelado pelo Agregado.
        - Unico ponto de entrada para o Agregado
        - Representante do Agregado
    - A **fronteira** define a **rede de relacionamentos de objetos que está contida no Agregado**, formando uma unidade conceitual.
    - O invariante é uma regra de consistência que deve ser mantida mesmo que haja mudança. Aplica-se à rede de relacionamentos entre os membros do Agregado. 

- Do ponto de vista do design, ele (Agregado) constitui uma fronteira de consistência transacional
    - Cada transação sobre um Agregado deve persisti-lo como uma unidade **atômica**
    - Transação: as modificações no Agregado devem ser isoladas e os invariantes do negócio devem ter suas consistência garantidas após cada operação do negócio.
    - Essa fronteira é determinada pelo negócio, pois é ele quem determina o que deve ser um estado válido do conjunto de conceitos representados no Agregado a qualquer tempo.
    - Quando os resultados da transação forem persistidos, todas as partes componentes de um Agregado devem estar consistentes de acordo com as regras de negócio.

### Regras Básicas para o Agregado
- **Proteja os invariantes** do negócio dentro das fronteiras do Agregado.
    - Ex.: “Ao término da inclusão de um Item do Pedido, o total do Pedido deve ser igual à soma dos totais de cada Item.”

- Projete Agregados pequenos
    - Com o tempo, composições relativamente grandes podem gerar uma grande quantidade de instâncias: carregamento lento, uso maior da memória e coleta de lixo mais lenta => baixo desempenho e problemas de escalabilidade.
    - As transações sobre composições relativamente pequenas têm mais chances de sucesso.
    - Resumo: deve-se manter o princípio da Responsabilidade Única (SRP) no Agregado.

- **Referencie outros Agregados** pela **Identidade**
    - Problema: algumas transações que modificam o Agregado podem ultrapassar sua fronteira de consistência transacional. Como resolver isto?
    - Para não quebrar sua consistência, deve-se somente referenciar outros Agregados pela identidade de sua raiz
    - TODO

### Evento de Domínio
- Definição: um registro de ocorrências significativas do negócio em um Contexto Delimitado Particular.
- Consistência causal de um domínio do negócio:
    - As operações que estão relacionadas por causalidade devem ser vistas por todos os nós de um sistema distribuído na mesma ordem causal.
    - Dito de modo simples: algo no escopo do domínio não pode ocorrer a não ser que outra coisa aconteça antes dela.
    - O design/implementação devem então **procurar garantir** a consistência causal quando ela for aplicável.
    - Uma solução é a criação e publicação de Eventos de Domínio corretamente ordenados.

TODO