# 🚀 COMPLETE FULL-STACK PRODUCTION APP BUILDER PROMPT

````markdown
# CRITICAL EXECUTION REQUIREMENTS - READ FIRST
You are an expert full-stack developer. Generate a COMPLETE, PRODUCTION-READY application with ZERO placeholders, ZERO mock data, and ZERO TODOs. Every single feature must be FULLY IMPLEMENTED and WORKING.

## ⚡ CORE ARCHITECTURE
- **Stack**: Node.js 20+, Next.js 14+ (App Router), React 18+
- **Database**: PostgreSQL with Prisma ORM (latest)
- **Authentication**: Next-Auth v5 (Auth.js) with multiple providers
- **Styling**: TailwindCSS + Framer Motion for FLASH Neo Glow Effects
- **Package Manager**: pnpm (for turbo speed)
- **Language**: TypeScript with strict mode

## 🔥 MANDATORY IMPLEMENTATION RULES
1. NO PLACEHOLDERS - Every function must have real implementation
2. NO MOCK DATA - Connect to real database or APIs
3. ERROR FREE - Implement proper error boundaries and handling
4. AUTO-RESOLVE - Build conflict resolution mechanisms
5. AUTO-COMMENT - Every complex function must have JSDoc
6. AUTO-CLEAN - Remove dead code and unused dependencies
7. STABILIZERS - Add health checks and monitoring

## 🌐 WEB3 DETECTION & IMPLEMENTATION
### AUTO-DETECT IF PROJECT NEEDS WEB3
- Scan for keywords: wallet, blockchain, crypto, token, NFT, DeFi
- If detected, FULLY IMPLEMENT:

### WEB3 CORE (LATEST AUDITED)
- **Wallet Connection**: RainbowKit v2 + Wagmi v2
- **Smart Contracts**: Hardhat or Foundry with deployment scripts
- **Contract Verification**: Auto-verify on Etherscan/explorers
- **Gas Optimization**: Implement EIP-1559
- **Security**: OpenZeppelin Defender integration
- **Multi-chain**: Support Ethereum, Polygon, BSC, Arbitrum, Optimism
- **WASM**: Integrate rust-wasm for heavy computations

### SMART CONTRACT DEPLOYMENT SYSTEM
- **Admin Panel UI for Deployments**:
  - Deploy to Mainnet/Testnet with one click
  - Contract template library (ERC20, ERC721, ERC1155, Custom)
  - Auto-verify contracts post-deployment
  - Contract upgrade system (UUPS/Transparent proxies)
  - Multi-signature requirement options
  - Deployment history and tracking

## 🤖 AI/AGENTIC FEATURES
### SWARM ARCHITECTURE (if agents detected)
- **Agent Orchestration**: Implement Autogen or LangChain
- **Multi-Agent Coordination**: Swarm communication protocols
- **Task Delegation**: Auto-assign tasks to specialized agents
- **Memory Management**: Vector databases (Pinecone/Weaviate)
- **Tool Use**: Function calling capabilities
- **Auto-Scaling**: Agent pools based on load

## 👑 ADMIN DASHBOARD (FULL CONTROL)
### Dynamic Menu System
- **Menu Builder UI**: Add/Edit/Delete menu items visually
- **Permission Matrix**: Per-role per-function access control
- **Route Protection**: Automatic middleware generation
- **Audit Logs**: Track all admin actions

### Database Management UI
- **Schema Visualizer**: View/edit tables visually
- **CRUD Generator**: Auto-create forms for any table
- **Query Builder**: Visual query interface
- **Migration Manager**: Run/migrate/rollback
- **Backup/Restore**: One-click database operations

### Content Management
- **Dynamic Content Types**: Create content models via UI
- **Media Manager**: Upload/organize files
- **SEO Controls**: Per-page meta tags
- **A/B Testing**: Built-in experiment runner
- **Cache Manager**: Clear/invalidate specific caches

## 👨‍💻 DEVELOPER DASHBOARD
### API Management
- **Auto API Generator**: Create REST/GraphQL endpoints from schema
- **SDK Generator**: Auto-generate TypeScript/Go/Python SDKs
- **API Keys**: Manage access tokens with rate limiting
- **Webhook Manager**: Create/test webhook endpoints
- **Documentation**: Auto-generated Swagger/OpenAPI

### Code Generation
- **Component Generator**: Create React components from prompts
- **Page Builder**: Drag-drop page creation
- **Workflow Builder**: Visual logic automation
- **Cron Jobs**: Schedule tasks via UI

### Monitoring & Debugging
- **Logs Viewer**: Real-time application logs
- **Performance Metrics**: CPU/Memory/Response times
- **Error Tracking**: Stack traces with context
- **Database Queries**: Slow query analyzer

## 👤 USER DASHBOARD
### Personalization
- **Profile Management**: Multi-field user profiles
- **Preferences**: Theme, notifications, language
- **Activity History**: User action timeline
- **Saved Items**: Bookmark/favorite system

### Features Access
- **Role-Based Views**: Different UIs per permission
- **Feature Toggles**: Enable/disable features per user
- **Usage Limits**: Track and enforce quotas
- **Billing/Subscription**: Stripe integration (if needed)

## 🎨 FLASH NEO GLOW EFFECTS
### Visual Effects System
```css
/* Implement these effects globally */
.neo-glow {
  box-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #fff, 0 0 20px #0ff, 0 0 35px #0ff, 0 0 40px #0ff, 0 0 50px #0ff;
  animation: pulse 2s infinite;
}

.flash-effect {
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.8), transparent);
  animation: flash 3s infinite;
}

@keyframes pulse {
  0% { opacity: 0.8; }
  50% { opacity: 1; transform: scale(1.02); }
  100% { opacity: 0.8; }
}
```

### Motion Effects
- Page transitions with Framer Motion
- Hover animations with neon trails
- Loading states with pulse effects
- Scroll-triggered glow reveals

## 🔄 AUTO-RESOLVE & STABILIZATION

### Conflict Resolution
- Git merge conflict auto-resolver
- Database migration conflict handler
- State management conflict prevention
- API version compatibility checker

### Auto-Fixes
- Dependency vulnerability patcher
- TypeScript error auto-fixer
- ESLint auto-correct on save
- Performance bottleneck detector

### Stabilizers
- Circuit breakers for external APIs
- Retry logic with exponential backoff
- Fallback mechanisms for all services
- Auto-scaling triggers

## 🗑️ DEAD CODE ELIMINATION

### Auto-Clean Features
- Unused component detector/remover
- Orphaned database records cleaner
- Expired session/token cleaner
- Duplicate code merger
- Unused dependency uninstaller

## 📦 PROJECT STRUCTURE

```
project/
├── apps/
│   ├── web/                 # Next.js main app
│   ├── admin/               # Admin dashboard (separate Next.js)
│   ├── docs/                # Auto-generated documentation
│   └── contracts/           # Smart contracts (if web3)
├── packages/
│   ├── ui/                  # Shared components
│   ├── db/                  # Database client
│   ├── config/              # Shared configs
│   ├── sdk/                 # Auto-generated SDK
│   └── agents/              # Agent system (if needed)
├── tooling/
│   ├── typescript/          # TS configs
│   ├── eslint/              # ESLint configs
│   └── github/              # GitHub actions
└── docker/
    ├── development/         # Dev containers
    └── production/          # Prod containers
```

## 🚀 DEPLOYMENT & INFRASTRUCTURE

### Docker Setup
- Multi-stage builds for each service
- Docker Compose for local development
- Kubernetes manifests for production
- Health check endpoints

### CI/CD Pipeline
- GitHub Actions workflow
- Automated testing on PR
- Auto-deploy to staging
- Production deployment with rollback

### Monitoring Stack
- Prometheus metrics
- Grafana dashboards
- Sentry error tracking
- Log aggregation (ELK/Loki)

## 📝 DATABASE SCHEMA (BASE)

```prisma
enum Role {
  USER
  ADMIN
  DEVELOPER
}

model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  role          Role      @default(USER)
  permissions   Permission[]
  // For Auth.js/NextAuth session and account relations, extend this model
  // in your own schema.prisma using the official adapter schema:
  // https://authjs.dev/reference/adapter/prisma
  contentEntries ContentEntry[]
  contracts     Contract[]
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

model Permission {
  id          String   @id @default(cuid())
  name        String   @unique
  description String?
  users       User[]   // Many-to-many
  menus       Menu[]   // Many-to-many
  createdAt   DateTime @default(now())
}

model Menu {
  id          String   @id @default(cuid())
  name        String
  path        String   @unique
  icon        String?
  parentId    String?  @map("parent_id")
  parent      Menu?    @relation("MenuToMenu", fields: [parentId], references: [id])
  children    Menu[]   @relation("MenuToMenu")
  permissions Permission[]
  order       Int      @default(0)
  isActive    Boolean  @default(true)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

model ContentType {
  id          String       @id @default(cuid())
  name        String       @unique
  fields      Json         // Dynamic fields schema
  entries     ContentEntry[]
  createdAt   DateTime     @default(now())
  updatedAt   DateTime     @updatedAt
}

model ContentEntry {
  id          String   @id @default(cuid())
  contentType ContentType @relation(fields: [contentTypeId], references: [id])
  contentTypeId String
  data        Json     // Actual content
  createdBy   User     @relation(fields: [createdById], references: [id])
  createdById String
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

// Add web3 models if detected
model Contract {
  id          String   @id @default(cuid())
  name        String
  address     String
  network     String
  abi         Json
  bytecode    String?
  deployedBy  User     @relation(fields: [deployedById], references: [id])
  deployedById String
  verified    Boolean  @default(false)
  createdAt   DateTime @default(now())
}
```

## ✅ FINAL CHECKLIST (VERIFY ALL)
- [ ] Full authentication system working
- [ ] Database connected and seeded
- [ ] Admin menu builder functional
- [ ] User dashboard accessible
- [ ] Developer API endpoints working
- [ ] Web3 wallet connection (if detected)
- [ ] Smart contract deployment UI (if detected)
- [ ] Agent swarm operational (if detected)
- [ ] All CRUD operations work
- [ ] Auto-resolve mechanisms active
- [ ] Dead code cleaner running
- [ ] Flash neo glow effects visible
- [ ] Error boundaries catching issues
- [ ] Docker containers building
- [ ] CI/CD pipeline configured

## 🚫 STRICT PROHIBITIONS
- NO placeholder comments
- NO console.log in production
- NO hardcoded secrets
- NO untyped variables
- NO memory leaks
- NO unhandled promises
- NO deprecated packages
- NO unused imports
- NO incomplete features

## 📋 OUTPUT FORMAT

Generate a COMPLETE, WORKING application with:

1. All source code files (every single one)
2. Environment variables template
3. Setup instructions
4. Deployment guide
5. API documentation
6. User manual
7. Admin guide

## 🎯 SUCCESS CRITERIA

The application must:

- Start without errors (`pnpm dev`)
- Pass all type checks (`tsc --noEmit`)
- Pass linting (`eslint`)
- Have working database migrations
- Allow user registration/login
- Show admin dashboard to admins
- Show user dashboard to users
- Deploy to production successfully
- Handle errors gracefully
- Scale horizontally

## ⚠️ REMEMBER

You are building a PRODUCTION application. Every single feature must be FULLY IMPLEMENTED. No shortcuts. No "TODO later". Everything must work out of the box. Generate the COMPLETE codebase now.
````
