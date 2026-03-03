Sistema de Gestão de Colaboradores e Documentos de Terceiros
Este projeto é uma aplicação web focada na gestão de documentos e cadastros de colaboradores terceirizados. Ele permite o acompanhamento de conformidade documental, aprovação de arquivos e controle de acessos por projetos de forma centralizada.

🚀 Principais Funcionalidades
Painel de Controle (Dashboard): Exibe KPIs de total de colaboradores, pendências documentais e cadastros aprovados.

Gestão de Funcionários: Cadastro de novos colaboradores com distinção entre contratação padrão e "Motorista RPA" (com exigências documentais diferentes).

Controle de Documentos:

Upload de arquivos (PDF e Imagens) diretamente pelo navegador.

Fluxo de aprovação: Status "Pendente", "Em Análise", "Aprovado", "Reprovado" ou "N/A" (Não Aplicável).

Feedback e histórico de auditoria por documento (quem enviou, quem revisou e quando).

Multiprojetos: Permite a criação de múltiplos projetos e o isolamento de colaboradores dentro do seu respetivo projeto.

🛠️ Stack Tecnológico
A aplicação é construída numa arquitetura Serverless e Single Page Application (SPA):

Frontend: HTML5, JavaScript (Vanilla ES6+), Tailwind CSS via CDN para estilização, e ícones Lucide.

Backend (BaaS): Firebase versão 12.6.0.

Arquitetura de Banco de Dados (Firestore)
A aplicação utiliza o Firebase Firestore (banco de dados NoSQL orientado a documentos) e está estruturada nas seguintes coleções principais:

Coleção usuarios: Gere o perfil e os acessos de quem usa o sistema.

email: E-mail de login do utilizador.

isAdmin: Booleano (true/false) que define privilégios globais.

projetosPermitidos: Array de strings contendo os IDs dos projetos aos quais os utilizadores não-administradores têm acesso.

Coleção projetos: Armazena as obras/contratos.

nome: Nome do projeto (ex: Avenida do Metrô).

createdAt: Data de criação no formato ISO.

Coleção funcionarios: Armazena os dados cadastrais e o estado dos documentos dos terceiros.

nome, funcao, empresa, cpf.

projetoId: Referência ao projeto onde o colaborador está alocado.

isDriverRPA: Booleano que aciona a lista específica de documentos para motoristas.

documentos: Um mapa (objeto) onde a chave é o ID do documento (ex: rg_cpf, aso) e o valor contém: url (Firebase Storage), status, mensagem (feedback), e dados de auditoria (uploadedAt, uploadedBy, reviewedAt, reviewedBy).

Armazenamento de Arquivos: O Firebase Storage é utilizado para guardar os ficheiros físicos enviados. Os arquivos são organizados no caminho: docs/{empId}/{docId}_{timestamp}.

Segurança e Controle de Acesso (RBAC)
O sistema implementa regras de autenticação e autorização nativas integradas à lógica de frontend:

Autenticação Segura: Gerida pelo Firebase Authentication, garantindo que apenas utilizadores com e-mail e palavra-passe válidos consigam carregar a aplicação.

Controle de Acesso Baseado em Papéis (RBAC): O sistema separa os utilizadores em dois perfis:

Administradores (isAdmin: true): Possuem acesso global a todos os projetos, podem criar novos projetos, gerir as permissões de acesso de outros utilizadores, excluir colaboradores e fazer aprovações/reprovações de documentos. Adicionalmente, administradores conseguem visualizar "Colaboradores Sem Projeto" (cadastros órfãos) e bases de legado.

Operadores / Utilizadores Comuns: Só podem visualizar e interagir com os projetos que lhes foram explicitamente atribuídos por um Administrador. Caso não tenham nenhum projeto associado, o sistema bloqueia o acesso aos dados exibindo o ecrã "Sem Projetos Disponíveis".

Auditoria de Documentos: Qualquer alteração num documento (upload ou avaliação) regista automaticamente na base de dados o e-mail do responsável e a data da ação (Timestamp).
