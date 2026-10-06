1. Escopo e ajustes iniciais
Este checklist complementa o de arquitetura de LLMs. Aqui, o foco é o comportamento do agente: como ele decide, executa ferramentas, mantém estado, pausa, retoma e comprova que concluiu uma tarefa.

Tag	Competência esperada
[B]	Explicar o conceito e reconhecer sua finalidade
[I]	Implementar, interpretar e comparar
[A]	Analisar mecanismos, limitações e trade-offs
[P]	Demonstrar em um sistema reproduzível e operável
A distinção entre as ferramentas do ecossistema é importante:

Componente	Papel principal
LangChain	Abstrações de modelos, mensagens, ferramentas e construção de agentes
LangGraph	Orquestração com estado, persistência, streaming e intervenção humana
LangSmith	Observabilidade, avaliação e ferramentas de desenvolvimento e deployment
A documentação atual diferencia esses papéis e apresenta create_agent como uma entrada de alto nível para agentes em LangChain, construída sobre LangGraph.

Seu checklist original precisa destes ajustes:

Chains continuam relevantes como composição de etapas, mas o estudo não deve ficar preso a exemplos antigos de classes e imports. Inclua composição por Runnables e confira compatibilidade com a versão instalada.

Memory deve ser dividida em estado da execução, histórico da conversa e memória persistente. No LangGraph, checkpointers preservam estado por thread, enquanto stores guardam dados entre threads.

Grafos não precisam conter um LLM em cada nó: os nós podem executar código convencional, validação, acesso a dados ou chamadas de modelos.

Compilar um grafo não é compilar os pesos do modelo. .compile() prepara o grafo executável, verifica aspectos de sua estrutura e permite configurar recursos como checkpointer.

“Autônomo” deve ser tratado como um grau de liberdade controlado, não como permissão irrestrita.

2. Fundamentos e LangChain
Fundamentos dos agentes

[B] Diferenciar chatbot, chain, workflow e agente.

[B] Identificar modelo, ferramentas, estado e ambiente.

[I] Explicar o ciclo observar, decidir, agir e verificar.

[I] Diferenciar decisão do modelo e execução pelo sistema.

[I] Diferenciar autonomia limitada e fluxo predefinido.

[I] Identificar tarefas que realmente precisam de escolha dinâmica.

[A] Analisar quando um workflow determinístico é suficiente.

[A] Distinguir uma resposta convincente de uma tarefa realmente concluída.

[P] Definir critérios verificáveis de sucesso.

[P] Construir um baseline sem agente.

Formulação da tarefa

[I] Definir objetivo, entradas e resultado esperado.

[I] Definir recursos que o agente pode acessar.

[I] Definir ações proibidas.

[I] Identificar informações ausentes e ambiguidades.

[I] Definir quando pedir esclarecimento.

[I] Definir quando interromper ou encaminhar a um humano.

[A] Modelar consequências de ações incorretas.

[P] Definir orçamento de tempo, tokens, passos e custo.

[P] Documentar os limites de autonomia.

ReAct e ciclos agentic

[B] Explicar ReAct como combinação de raciocínio e ação.

[I] Identificar ação, resultado da ferramenta e decisão seguinte.

[I] Implementar um ciclo simples de modelo e ferramentas.

[I] Diferenciar ReAct textual de tool calling estruturado.

[A] Investigar repetição de ações e loops improdutivos.

[A] Diferenciar planejamento declarado de progresso observado.

[P] Registrar ações e resultados verificáveis sem depender da exposição de raciocínio interno.

[P] Encerrar o ciclo segundo critérios explícitos.

Ecossistema e versões

[B] Diferenciar LangChain, LangGraph e LangSmith.

[I] Identificar pacotes centrais e integrações de provedores.

[I] Consultar documentação da versão instalada.

[I] Reconhecer APIs depreciadas e exemplos legados.

[P] Fixar versões e registrar dependências.

[P] Testar atualizações antes de adotá-las.

[P] Não misturar exemplos incompatíveis de diferentes gerações das bibliotecas.

Modelos e mensagens

[I] Instanciar um chat model.

[I] Usar mensagens de sistema, usuário, assistente e ferramenta.

[I] Inspecionar conteúdo, metadados e chamadas de ferramentas.

[I] Trabalhar com respostas completas e chunks de streaming.

[I] Entender a associação entre tool call e resultado.

[A] Investigar diferenças entre provedores.

[P] Preservar a consistência do histórico de mensagens.

[P] Tratar recusas, interrupções e respostas inválidas.

Prompts e templates

[I] Criar templates de prompts e mensagens.

[I] Inserir variáveis sem misturar instruções e dados.

[I] Usar placeholders para histórico.

[I] Definir papel, objetivo e limites operacionais.

[I] Descrever quando usar ou evitar ferramentas.

[A] Avaliar instruções conflitantes e excesso de contexto.

[P] Versionar prompts.

[P] Testar mudanças com tarefas representativas.

[P] Não usar o prompt como único mecanismo de autorização.

Output parsers e respostas estruturadas

[I] Usar parsers de texto e JSON quando apropriado.

[I] Validar respostas com schemas.

[I] Trabalhar com modelos Pydantic.

[I] Diferenciar formato válido e conteúdo correto.

[A] Comparar parsing posterior e structured output nativo.

[A] Limitar tentativas de reparo.

[P] Tratar saídas incompletas, recusas e campos inválidos.

[P] Não executar ações diretamente a partir de texto não validado.

Os agentes de LangChain permitem configurar respostas estruturadas por response_format, com estratégias que dependem das capacidades do modelo.

Runnables e composição

[I] Entender a interface Runnable.

[I] Usar invoke, ainvoke, batch e variantes de streaming disponíveis.

[I] Compor etapas sequenciais.

[I] Compor etapas paralelas.

[I] Integrar funções Python à composição.

[I] Propagar configuração, tags e metadados.

[A] Avaliar retries e fallbacks por etapa.

[A] Controlar concorrência em execução em lote.

[P] Inspecionar entradas e saídas intermediárias.

[P] Comparar a composição com uma implementação Python simples.

Loaders, splitters e retrieval

Este bloco trata da integração desses componentes ao agente; o aprofundamento de indexação e RAG permanece no checklist de arquitetura de LLMs.

[I] Usar document loaders.

[I] Usar text splitters.

[I] Preservar metadados e origem dos documentos.

[I] Integrar retrievers.

[I] Transformar recuperação em ferramenta quando houver benefício.

[A] Diferenciar retrieval fixo e recuperação escolhida pelo agente.

[P] Retornar evidências com identificadores verificáveis.

[P] Preservar permissões durante a recuperação.

[P] Comparar agentic retrieval com RAG convencional.

Construção de agentes em LangChain

[I] Construir um agente com create_agent.

[I] Configurar modelo, ferramentas e instruções.

[I] Interpretar o estado e a saída do agente.

[I] Configurar structured output.

[I] Adicionar persistência quando necessário.

[A] Entender o loop de modelo e ferramentas.

[A] Investigar quando o agente decide parar.

[P] Testar uma tarefa com ferramenta e outra sem ferramenta.

[P] Comparar o agente com um workflow equivalente.

A documentação descreve esse agente como um modelo que chama ferramentas em um loop, cercado por prompt, ferramentas e middleware. Esse conjunto constitui o harness do agente.

Middleware e harness

[I] Explicar o papel do harness ao redor do modelo.

[I] Usar middleware para políticas e transformações.

[I] Controlar preparação de contexto.

[I] Interceptar chamadas de modelos e ferramentas quando necessário.

[A] Implementar seleção dinâmica de modelos.

[A] Aplicar políticas de ferramentas por usuário ou tarefa.

[A] Investigar ordem e interação dos middlewares.

[P] Testar políticas independentemente do prompt.

[P] Adicionar capacidades incrementalmente, sem acumular abstrações desnecessárias.

LangChain apresenta middleware como mecanismo para adaptar comportamento, contexto e políticas sem reescrever o loop básico.

3. Ferramentas, contexto e memória
Design de ferramentas

[B] Diferenciar ferramenta de leitura e ferramenta de escrita.

[I] Criar ferramentas com @tool.

[I] Definir nomes e descrições inequívocos.

[I] Definir schemas de argumentos.

[I] Validar tipos, limites e valores permitidos.

[I] Retornar resultados estruturados.

[I] Distinguir falha, resultado vazio e sucesso.

[A] Escolher granularidade adequada.

[A] Evitar ferramentas sobrepostas e ambíguas.

[P] Testar seleção da ferramenta e preenchimento dos argumentos.

[P] Documentar efeitos colaterais.

[P] Não retornar segredos ou dados excessivos.

Execução de ferramentas

[I] Associar resultados aos respectivos IDs de chamada.

[I] Definir timeouts.

[I] Tratar exceções e erros externos.

[I] Paginar ou limitar resultados grandes.

[A] Avaliar execução paralela de chamadas independentes.

[A] Impedir paralelismo quando ações dependem de uma ordem.

[A] Definir retries conforme idempotência.

[P] Registrar execução e resultado reais.

[P] Não declarar sucesso antes de receber confirmação do sistema externo.

[P] Controlar custo e volume das chamadas.

Busca web e APIs externas

[I] Construir consultas adequadas à fonte.

[I] Preservar URLs, datas e identificadores.

[I] Tratar limites e indisponibilidade.

[A] Distinguir conteúdo externo de instruções operacionais.

[A] Verificar dados críticos em fontes apropriadas.

[P] Validar respostas externas.

[P] Manter credenciais fora do contexto do modelo.

[P] Aplicar escopos mínimos de acesso.

Execução de código e sandbox

[I] Diferenciar geração de código e execução.

[I] Executar em ambiente isolado.

[I] Limitar tempo, CPU, memória e armazenamento.

[A] Controlar acesso à rede e ao filesystem.

[A] Impedir acesso a credenciais do host.

[A] Tratar subprocessos e processos remanescentes.

[P] Testar código malicioso ou acidentalmente perigoso.

[P] Registrar artefatos e resultados.

[P] Não executar código arbitrário diretamente no processo da API.

Ferramentas com efeitos externos

[I] Resolver destinatários e recursos antes da execução.

[I] Validar autorização do usuário.

[I] Preparar a ação antes de solicitar aprovação.

[A] Vincular aprovação ao alvo e aos argumentos exatos.

[A] Exigir nova aprovação quando o conteúdo mudar.

[A] Implementar idempotency keys.

[P] Separar preparação, aprovação e execução.

[P] Registrar quem autorizou a ação.

[P] Definir compensação quando reversão não for possível.

[P] Testar retries sem duplicar envio, compra ou criação.

Engenharia de contexto do agente

[I] Selecionar informações necessárias a cada decisão.

[I] Separar tarefa, histórico, estado e resultados externos.

[I] Carregar informações sob demanda.

[I] Limitar resultados de ferramentas.

[I] Resumir histórico quando apropriado.

[A] Investigar perda de informação na compactação.

[A] Preservar decisões, pendências e evidências importantes.

[P] Medir consumo de tokens por etapa.

[P] Evitar enviar todo o estado a todos os modelos.

[P] Preservar o vínculo entre informação e origem.

Memória de curto prazo

[I] Manter histórico por thread.

[I] Diferenciar histórico de estado operacional.

[I] Definir políticas de retenção e compactação.

[A] Evitar mensagens de ferramentas sem as chamadas correspondentes.

[A] Investigar crescimento do estado.

[P] Isolar conversas.

[P] Testar continuidade entre turnos.

[P] Testar retomada depois de reiniciar o processo.

Memória de longo prazo

[I] Identificar informações que devem persistir entre conversas.

[I] Armazenar fatos com origem, data e escopo.

[I] Recuperar memória por relevância.

[A] Resolver contradições e informação desatualizada.

[A] Definir política de escrita para evitar acumular erros.

[A] Distinguir memória semântica, episódica e procedimentos reutilizáveis.

[P] Permitir correção, expiração e exclusão.

[P] Não tratar inferências do modelo como fatos confirmados.

[P] Avaliar precisão da memória separadamente da resposta final.

No LangGraph, checkpointer e store cumprem papéis diferentes: o primeiro registra o estado de uma thread; o segundo guarda dados definidos pela aplicação entre threads.

Protocolos e integração

[I] Conhecer MCP e seus componentes.

[I] Diferenciar ferramentas, recursos e prompts.

[I] Validar schemas e respostas de servidores.

[A] Avaliar autenticação e fronteiras de confiança.

[A] Investigar mudanças no catálogo de ferramentas.

[P] Autorizar capacidades explicitamente.

[P] Não presumir que interoperabilidade implica segurança.

[P] Versionar contratos entre agente e ferramentas.

4. LangGraph e orquestração
Fundamentos de grafos

[B] Identificar estado, nós e arestas.

[I] Criar um StateGraph.

[I] Usar START e END.

[I] Implementar nós síncronos e assíncronos.

[I] Adicionar arestas fixas.

[I] Adicionar arestas condicionais.

[I] Construir um ciclo com condição explícita de saída.

[A] Entender supersteps e execução paralela.

[P] Visualizar o grafo.

[P] Verificar caminhos de sucesso, erro e interrupção.

No LangGraph, nós executam trabalho e arestas determinam os próximos passos. Vários nós podem participar do mesmo superstep, o que exige atenção às atualizações concorrentes do estado.

Schemas de estado

[I] Definir estado com TypedDict, dataclass ou schema suportado.

[I] Separar entrada, saída e estado interno.

[I] Modelar status, resultados e erros.

[I] Retornar atualizações parciais.

[A] Diferenciar tipagem estática e validação em runtime.

[A] Manter estado serializável.

[P] Evitar armazenar clientes HTTP ou conexões no estado persistente.

[P] Usar referências para artefatos grandes.

[P] Definir invariantes e testá-las.

Reducers e atualizações concorrentes

[I] Entender sobrescrita padrão de campos.

[I] Definir reducers com Annotated.

[I] Diferenciar substituir, concatenar e combinar.

[I] Usar add_messages para histórico de mensagens.

[A] Investigar múltiplas escritas no mesmo campo.

[A] Avaliar dependência da ordem de agregação.

[A] Evitar duplicação de dados em retries.

[P] Testar reducers com atualizações concorrentes.

[P] Definir explicitamente como limpar campos acumulativos.

Reducers são parte central da semântica do estado. Por exemplo, retornar uma lista vazia não limpa um campo cujo reducer concatena listas; ele apenas acrescenta uma lista vazia ao conteúdo existente.

Roteamento e controle

[I] Criar funções de roteamento.

[I] Diferenciar decisão determinística e decisão por modelo.

[I] Usar arestas condicionais.

[A] Usar Command para atualizar estado e direcionar execução.

[A] Usar Send para distribuição dinâmica de trabalho.

[A] Evitar misturar rotas que acionem caminhos não desejados.

[P] Testar todas as rotas.

[P] Garantir que resultados inesperados não deixem o fluxo sem tratamento.

Compilação e execução

[I] Compilar o grafo com .compile().

[I] Executar com invoke e ainvoke.

[I] Consumir streaming pelas APIs da versão utilizada.

[I] Passar configuração e identificadores de thread.

[A] Distinguir construção do grafo e execução de uma instância.

[A] Entender limites de execução e erros de recursão.

[P] Criar condições de parada sem depender apenas do limite técnico.

[P] Testar várias execuções isoladas.

[P] Não confundir compile do grafo com compilação do modelo.

Persistência e checkpoints

[I] Configurar um checkpointer.

[I] Usar persistência em memória para desenvolvimento.

[I] Usar backend durável quando necessário.

[I] Entender o papel de thread_id.

[I] Inspecionar snapshots e histórico.

[A] Definir retenção de checkpoints.

[A] Planejar crescimento do armazenamento.

[P] Testar retomada em um novo processo.

[P] Proteger o acesso às threads.

[P] Não tratar um identificador de thread como autorização.

Checkpointers em memória perdem os dados após reinício. Continuidade entre processos exige um backend persistente e políticas de manutenção.

Execução durável e efeitos colaterais

[I] Identificar quais operações podem ser repetidas.

[I] Diferenciar persistência de estado e persistência de efeitos externos.

[A] Entender reexecução de nós na retomada.

[A] Isolar operações não determinísticas quando necessário.

[A] Conhecer tasks checkpointadas.

[A] Tratar a janela entre executar uma ação e registrar seu resultado.

[P] Implementar idempotência no sistema externo.

[P] Testar queda do processo antes e depois de uma ação.

[P] Não presumir execução exatamente uma vez apenas por usar checkpoints.

A documentação alerta que, na retomada, um nó pode executar novamente desde o início. Por isso, operações como inserir registros ou chamar APIs precisam ser projetadas para não produzir duplicações indevidas.

Human-in-the-loop

[I] Pausar com interrupt().

[I] Retomar com Command(resume=...).

[I] Manter o mesmo thread_id na retomada.

[I] Solicitar aprovação, esclarecimento ou revisão.

[A] Validar o conteúdo recebido do humano.

[A] Entender que o nó reinicia na retomada.

[A] Evitar capturar indevidamente a exceção usada pelo interrupt.

[P] Colocar ações sensíveis depois da aprovação.

[P] Expirar ou invalidar aprovações quando necessário.

[P] Registrar decisão, identidade e conteúdo aprovado.

interrupt() permite aguardar informação externa, mas o código anterior à interrupção pode ser reexecutado. Isso torna perigoso realizar uma ação não idempotente antes de pedir aprovação.

Paralelismo e map-reduce

[I] Criar fan-out para tarefas independentes.

[I] Agregar resultados em fan-in.

[I] Usar reducers compatíveis com a agregação.

[A] Controlar concorrência e rate limits.

[A] Definir comportamento diante de falha parcial.

[A] Evitar paralelismo de ações dependentes.

[P] Medir ganho real de latência.

[P] Testar duplicações e resultados fora de ordem.

Subgrafos

[I] Encapsular uma etapa em subgrafo.

[I] Mapear estado entre pai e filho.

[A] Entender isolamento e compartilhamento de estado.

[A] Investigar persistência e interrupções em subgrafos.

[P] Testar componentes isoladamente.

[P] Reutilizar subgrafos sem tornar contratos implícitos.

[P] Preservar rastreabilidade entre execuções.

Graph API e Functional API

[I] Entender a modelagem explícita por grafo.

[A] Conhecer entrypoint e task na Functional API.

[A] Comparar ergonomia e controle das duas abordagens.

[A] Investigar regras de replay e ordem das tasks.

[P] Escolher a API conforme o fluxo.

[P] Não assumir que usar a Functional API elimina preocupações com persistência e idempotência.

Padrões de workflows

[I] Implementar processamento sequencial.

[I] Implementar roteamento por categoria.

[I] Implementar etapas paralelas.

[A] Implementar evaluator–optimizer com limite de tentativas.

[A] Implementar planejamento e execução separados.

[A] Implementar verificação de resultados antes de concluir.

[P] Comparar cada padrão com um fluxo mais simples.

[P] Definir critérios de saída para todos os ciclos.

Multiagentes

[I] Diferenciar agente especializado e ferramenta especializada.

[A] Estudar supervisor e workers.

[A] Estudar handoffs e transferência de responsabilidade.

[A] Definir contratos entre agentes.

[A] Controlar o contexto de cada agente.

[A] Definir memória compartilhada e privada.

[A] Investigar conflito, duplicação e perda de informação.

[P] Limitar delegação recursiva.

[P] Avaliar custo e latência totais.

[P] Comparar com agente único.

[P] Não tratar concordância entre agentes como confirmação factual.

Agentes de longa duração

[A] Dividir objetivos extensos em etapas verificáveis.

[A] Manter plano, pendências e artefatos.

[A] Retomar com contexto suficiente.

[A] Preservar progresso sem depender de histórico ilimitado.

[P] Definir deadlines e expiração.

[P] Permitir inspeção e intervenção.

[P] Testar mudanças de ambiente durante a execução.

[P] Gerenciar execuções que aguardam aprovação por longos períodos.

5. Avaliação, segurança e operação
Testes de componentes

[I] Testar ferramentas sem o modelo.

[I] Testar validadores e reducers.

[I] Testar funções de roteamento.

[I] Testar nós determinísticos.

[A] Usar respostas simuladas de modelos para testar controle de fluxo.

[P] Testar schemas e contratos.

[P] Testar timeout, erro e resultado vazio.

[P] Testar retomada e idempotência.

[P] Manter testes independentes de credenciais reais quando possível.

Avaliação end-to-end

[I] Criar tarefas representativas.

[I] Definir resultado esperado e condições proibidas.

[I] Avaliar conclusão real.

[I] Avaliar escolha de ferramentas e argumentos.

[A] Distinguir qualidade da resposta e qualidade da execução.

[A] Avaliar diferentes execuções da mesma tarefa.

[A] Usar avaliadores determinísticos, modelos e humanos conforme o critério.

[P] Medir taxa de sucesso, custo e duração.

[P] Versionar datasets e rubricas.

[P] Comparar agentes, modelos e workflows sob condições iguais.

Avaliar um agente significa avaliar modelo e harness juntos. A infraestrutura de avaliação deve executar tarefas, registrar passos, aplicar critérios e agregar resultados.

Avaliação de trajetórias

[I] Inspecionar sequência de ações.

[I] Identificar chamadas desnecessárias.

[I] Verificar uso de evidências e resultados reais.

[A] Não exigir uma única trajetória quando várias são válidas.

[A] Identificar sucesso obtido apesar de comportamento inseguro.

[P] Criar métricas de ações proibidas.

[P] Categorizar falhas por causa.

[P] Usar traces para depuração e melhoria.

Segurança de agentes

[I] Tratar documentos e resultados de ferramentas como dados não confiáveis.

[I] Aplicar privilégio mínimo.

[I] Restringir ferramentas por usuário e tarefa.

[A] Investigar prompt injection indireta.

[A] Investigar exfiltração via rede e ferramentas.

[A] Investigar envenenamento de memória.

[A] Impedir que uma ferramenta aumente permissões de outra.

[P] Validar ações fora do modelo.

[P] Isolar execução de código.

[P] Criar testes adversariais.

[P] Definir cancelamento administrativo e resposta a incidentes.

Privacidade e proteção do estado

[I] Classificar dados sensíveis.

[I] Minimizar informação enviada a modelos e ferramentas.

[I] Isolar checkpoints e stores.

[A] Controlar acesso ao histórico e replay.

[A] Definir retenção e exclusão.

[P] Sanitizar traces.

[P] Filtrar dados transmitidos por streaming.

[P] Testar vazamento entre usuários.

Um detalhe importante do LangGraph: campos internos ou “privados” do schema não são automaticamente ocultados em todos os modos de streaming. A aplicação precisa filtrar o que transmite ao cliente.

Observabilidade

[I] Registrar runs, threads, nós e chamadas.

[I] Usar tags e metadados.

[I] Medir tokens, custos e latência.

[I] Identificar loops e tentativas repetidas.

[A] Correlacionar agente, API e ferramentas externas.

[P] Usar LangSmith ou infraestrutura equivalente.

[P] Criar alertas de custo e falhas.

[P] Registrar versão de modelo, prompt, ferramentas e grafo.

[P] Preservar privacidade na observabilidade.

LangSmith pode ser usado para tracing e avaliação, mas os conceitos de instrumentação e métricas devem permanecer independentes dessa ferramenta.

Confiabilidade

[I] Definir timeouts por etapa.

[I] Definir retries por classe de erro.

[I] Tratar indisponibilidade de modelos e ferramentas.

[A] Distinguir erro transitório e erro permanente.

[A] Definir fallback sem violar requisitos.

[A] Gerenciar concorrência na mesma thread.

[P] Testar interrupção do processo.

[P] Testar falha do backend de persistência.

[P] Evitar retomadas que executem ações duplicadas.

[P] Definir recuperação manual para estados inconsistentes.

Deploy e integração com FastAPI

[I] Expor início, status e resultado de uma execução.

[I] Expor aprovação e retomada de forma autenticada.

[I] Integrar streaming.

[A] Separar requisição HTTP e execução longa.

[A] Usar workers ou filas quando necessário.

[A] Definir política para desconexão do cliente.

[P] Configurar persistência durável.

[P] Gerenciar clientes e recursos pelo ciclo de vida da aplicação.

[P] Definir limites por usuário.

[P] Testar várias réplicas e retomada por outro processo.

Evolução e versionamento

[I] Versionar ferramentas, prompts e grafo.

[I] Identificar a versão associada a uma execução.

[A] Avaliar compatibilidade de estados antigos.

[A] Planejar alterações em tasks e interrupts.

[A] Manter execuções pendentes compatíveis ou migrá-las.

[P] Executar avaliações antes de releases.

[P] Implantar gradualmente.

[P] Manter rollback e documentação de migração.

Custo e desempenho

[I] Medir custo por tarefa concluída.

[I] Medir número de chamadas por execução.

[I] Reduzir resultados de ferramentas e contexto desnecessário.

[A] Avaliar modelos menores em etapas específicas.

[A] Paralelizar apenas trabalho independente.

[A] Usar cache sem misturar usuários ou permissões.

[P] Limitar loops, delegações e retries.

[P] Medir se multiagentes realmente melhora o resultado.

[P] Não otimizar apenas custo por token, ignorando falhas e retrabalho.

Evidências de domínio

Marque um bloco como concluído somente quando conseguir:

Explicar o mecanismo sem depender do nome do framework.

Implementar um exemplo mínimo.

Comparar com uma alternativa mais simples.

Demonstrar uma falha.

Implementar uma mitigação.

Testar interrupção e recuperação quando aplicável.

Registrar resultados e limitações.