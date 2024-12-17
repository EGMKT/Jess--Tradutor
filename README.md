# **Sistema de Tradução de PDF**

Uma aplicação web moderna para tradução de documentos PDF com funcionalidades avançadas de gerenciamento de bases de conhecimento.

---

## 🌟 **Funcionalidades**

### **Tradução de Documentos**
- Upload de arquivos PDF para tradução.
- Suporte a múltiplos idiomas (especificar lista de idiomas suportados, se houver).
- Acompanhamento em tempo real do progresso da tradução.
- Validação de tamanho e formato de arquivo (detalhar tamanhos máximos permitidos e formatos aceitos).
- Opção de download dos documentos traduzidos.

### **Gerenciamento de Base de Conhecimento**
- Criação e gerenciamento de glossários de tradução.
- Importação e exportação de entradas do glossário via arquivos CSV.
- Organização das entradas por categorias (exemplo: Jurídico, Técnico, Geral).
- Traduções com reconhecimento de contexto (adicionar detalhes sobre como o contexto é considerado).
- Suporte multilíngue para bases de conhecimento.

### **Autenticação de Usuários**
- Sistema seguro de login.
- Autenticação baseada em JWT (JSON Web Token).
- Proteção de rotas e endpoints da API.

---

## 🏗 **Estrutura do Projeto**

### **Frontend (`/src`)**

#### **Componentes**
- **auth/**
  - `LoginForm.tsx` - Formulário de login e autenticação do usuário.
  
- **upload/**
  - `FileUpload.tsx` - Componente de upload de arquivos PDF com suporte a drag-and-drop.
  
- **translation/**
  - `LanguageSelector.tsx` - Menu dropdown para seleção de idioma.
  - `TranslationProgress.tsx` - Componente de acompanhamento do progresso da tradução.
  - `TranslatedDocuments.tsx` - Lista de traduções concluídas com opção de download.

- **knowledge/**
  - `GlossaryEditor.tsx` - Editor para modificar entradas do glossário.
  - `GlossaryFileUpload.tsx` - Componente para importar glossários a partir de arquivos CSV.
  - `KnowledgeBaseForm.tsx` - Formulário para criar novas bases de conhecimento.
  - `KnowledgeBaseList.tsx` - Navegação e gerenciamento das bases de conhecimento existentes.

#### **Utilitários**
- `fileParser.ts` - Funções utilitárias para análise e manipulação de arquivos.
- `types/index.ts` - Definições de tipos para uso no TypeScript.

---

### **Backend (`/server`)**

#### **Rotas da API**
- `auth.routes.ts` - Endpoints relacionados à autenticação.
- `translation.routes.ts` - Endpoints de gerenciamento de traduções.
- `knowledge.routes.ts` - Endpoints de operações em bases de conhecimento.

#### **Controladores**
- `auth.controller.ts` - Lógica para autenticação e controle de usuários.
- `translation.controller.ts` - Processamento de traduções e gerenciamento de arquivos.
- `knowledge.controller.ts` - Gerenciamento de bases de conhecimento e glossários.

#### **Serviços**
- `translation.service.ts` - Lógica central do processamento de traduções.

#### **Configurações**
- `database.ts` - Configuração do banco de dados utilizando Prisma.
- `env.ts` - Gerenciamento das variáveis de ambiente.

#### **Middlewares**
- `auth.ts` - Middleware para autenticação JWT.
- `error.ts` - Middleware para tratamento de erros globais.

---

### **Esquema do Banco de Dados (`/server/prisma`)**

#### Entidades Principais
```prisma
model User {
  id           Int    @id @default(autoincrement())
  email        String @unique
  password     String
  createdAt    DateTime @default(now())
  translations Translation[]
}

model Translation {
  id           Int      @id @default(autoincrement())
  userId       Int
  fileName     String
  status       String   // Exemplo: "Em progresso", "Concluído"
  language     String
  translatedAt DateTime @default(now())
  user         User     @relation(fields: [userId], references: [id])
}

model KnowledgeBase {
  id           Int      @id @default(autoincrement())
  name         String
  createdAt    DateTime @default(now())
  glossary     GlossaryEntry[]
}

model GlossaryEntry {
  id            Int      @id @default(autoincrement())
  knowledgeBaseId Int
  term          String
  translation   String
  category      String   // Exemplo: "Jurídico", "Técnico"
  knowledgeBase KnowledgeBase @relation(fields: [knowledgeBaseId], references: [id])
}
```

---

## 🛠 **Tecnologias Utilizadas**

### **Frontend**
- **React 18** - Biblioteca principal para desenvolvimento de interfaces.
- **TypeScript** - Tipagem estática para maior robustez.
- **Tailwind CSS** - Estilização com utilitários.
- **Vite** - Ferramenta rápida de build.
- **Lucide React** - Biblioteca de ícones.

### **Backend**
- **Node.js** - Ambiente de execução JavaScript no servidor.
- **Express** - Framework minimalista para APIs.
- **Prisma** - ORM moderno para gerenciar o banco de dados PostgreSQL.
- **JWT** - Gerenciamento de tokens para autenticação segura.

### **Ferramentas de Desenvolvimento**
- ESLint - Análise de código para padrões consistentes.
- Prettier - Formatação automática do código.
- Zod - Validação de entradas e dados.

---

## 🚀 **Passo a Passo para Inicialização**

1. **Clone o repositório**:
```bash
git clone <URL-do-repositorio>
cd <nome-do-projeto>
```

2. **Instale as dependências**:
```bash
npm install
```

3. **Configure as variáveis de ambiente**:
Crie um arquivo `.env` na raiz do projeto e insira:
```env
DATABASE_URL="postgresql://usuario:senha@localhost:5432/pdf_translator"
JWT_SECRET="sua-chave-secreta"
PORT=3000
UPLOAD_DIR="./uploads"
```

4. **Inicie os servidores**:
- **Frontend**:
```bash
npm run dev
```
- **Backend**:
```bash
npm run server:dev
```

---

## 📝 **Endpoints da API**

### **Autenticação**
- `POST /api/auth/login` - Autenticar usuário e retornar token JWT.

### **Traduções**
- `POST /api/translations` - Fazer upload de um PDF para tradução.
- `GET /api/translations` - Listar traduções do usuário.
- `GET /api/translations/:id` - Obter detalhes de uma tradução específica.

### **Bases de Conhecimento**
- `POST /api/knowledge-bases` - Criar uma nova base de conhecimento.
- `GET /api/knowledge-bases` - Listar bases de conhecimento.
- `POST /api/knowledge-bases/:id/entries` - Adicionar entrada ao glossário.

---

## 🔒 **Recursos de Segurança**

- Autenticação baseada em JWT.
- Hashing de senhas com **bcrypt**.
- Validação de entradas usando **Zod**.
- Restrições para upload de arquivos.
- Configuração de CORS.

---

## 🎯 **Melhorias Futuras**

1. Sistema de registro de usuários.
2. Integração com OAuth (Google, Facebook).
3. Processamento avançado de PDFs (OCR).
4. Suporte a traduções em lote.
5. Integração com Machine Learning para traduções mais precisas.
6. Colaboração em tempo real.
7. Sistema de memória de tradução.

