# SocialAi Technical Specification

## Overview

SocialAi is a lightweight, AI-powered social discovery engine built on a **parallel, auto-healing, one-file node architecture** powered by Healdec and SmartBrain. It mirrors public Farcaster activity, blends optional Reddit timelines, and exposes SEO-optimized public profiles that users can claim by verifying their Farcaster identity.

---

## Frameworks

### Frontend - Public Layer
- **Astro**: Static site generation and server-side rendering framework
  - Zero-JS by default for optimal performance
  - SEO-optimized pages for profiles, posts, and timelines
  - Fast SSR (Server-Side Rendering) for dynamic content
  - Handles landing pages and claim flow UI

- **Vite**: Frontend build tool and development server
  - Hot module replacement for development
  - Optimized production builds
  - Asset optimization and bundling

### Frontend - Admin Layer
- **Angular**: Full-featured framework for admin console
  - Feature flag management interface
  - Sync control dashboard
  - Worker monitoring and health checks
  - System administration tools

### Backend
- **Node.js**: Runtime environment
  - One-file node architecture
  - High-performance async I/O
  - Healdec engine integration

### Database
- **PostgreSQL**: Primary data store
  - ACID compliance
  - Advanced indexing for performance
  - Vector extension support for embeddings
  - Full-text search capabilities

---

## Workers

The SocialAi backend operates on a **parallel, auto-healing worker architecture** powered by the Healdec engine. Each worker runs independently and can self-recover from failures.

### Worker Types

#### 1. Chain Workers
- **Purpose**: Parallel blockchain data ingestion
- **Responsibilities**:
  - Connect to Farcaster Hubs
  - Ingest casts, profiles, and social graph data
  - Process blockchain events in real-time
  - Maintain sync state

#### 2. AI Worker
- **Purpose**: Machine learning and embeddings processing
- **Responsibilities**:
  - Generate text embeddings for posts and profiles
  - Create timeline summaries
  - Process profile optimization suggestions
  - Generate topic clusters
  - Calculate recommendations

#### 3. Search Worker
- **Purpose**: Full-text and vector search indexing
- **Responsibilities**:
  - Index posts and profiles for search
  - Maintain vector embeddings index
  - Handle search queries
  - Update search rankings

#### 4. Sync Worker
- **Purpose**: External data synchronization
- **Responsibilities**:
  - Reddit timeline sync (optional)
  - Future: Lens, Bluesky, Zora integration
  - Rate limiting and API management
  - Data normalization

#### 5. RPC Workers
- **Purpose**: Handle API requests
- **Responsibilities**:
  - Process REST API calls
  - Handle authentication
  - Manage user sessions
  - Execute database queries
  - Return formatted responses

### Worker Features
- **Auto-healing**: Workers automatically restart on failure
- **Parallel execution**: Workers run concurrently for maximum throughput
- **State management**: Each worker maintains its own state
- **Health monitoring**: Continuous health checks and reporting
- **Load balancing**: Dynamic work distribution

---

## Database Schema

### Core Tables

#### users
- `id` (UUID, Primary Key)
- `wallet_address` (String, Unique)
- `ens_name` (String, Nullable)
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

#### profiles
- `id` (UUID, Primary Key)
- `user_id` (UUID, Foreign Key → users.id, Nullable)
- `farcaster_fid` (Integer, Unique)
- `username` (String, Unique)
- `display_name` (String)
- `bio` (Text)
- `avatar_url` (String)
- `verified` (Boolean)
- `claimed` (Boolean)
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

#### posts
- `id` (UUID, Primary Key)
- `profile_id` (UUID, Foreign Key → profiles.id)
- `content` (Text)
- `cast_hash` (String, Unique, Nullable)
- `parent_hash` (String, Nullable)
- `created_at` (Timestamp)
- `indexed_at` (Timestamp)

#### external_posts
- `id` (UUID, Primary Key)
- `source` (Enum: reddit, lens, bluesky, zora)
- `source_id` (String)
- `author` (String)
- `content` (Text)
- `url` (String)
- `created_at` (Timestamp)
- `synced_at` (Timestamp)
- Unique constraint: (source, source_id) - prevents duplicate posts from the same external source

#### follows
- `id` (UUID, Primary Key)
- `follower_id` (UUID, Foreign Key → profiles.id)
- `following_id` (UUID, Foreign Key → profiles.id)
- `created_at` (Timestamp)
- Unique constraint: (follower_id, following_id)

#### likes
- `id` (UUID, Primary Key)
- `profile_id` (UUID, Foreign Key → profiles.id)
- `post_id` (UUID, Foreign Key → posts.id)
- `created_at` (Timestamp)
- Unique constraint: (profile_id, post_id)

#### saved_posts
- `id` (UUID, Primary Key)
- `profile_id` (UUID, Foreign Key → profiles.id)
- `post_id` (UUID, Foreign Key → posts.id)
- `created_at` (Timestamp)
- Unique constraint: (profile_id, post_id)

#### claims
- `id` (UUID, Primary Key)
- `profile_id` (UUID, Foreign Key → profiles.id, Unique)
- `user_id` (UUID, Foreign Key → users.id)
- `signature` (Text)
- `verified_at` (Timestamp)
- `created_at` (Timestamp)

#### embeddings
- `id` (UUID, Primary Key)
- `entity_type` (Enum: post, profile)
- `entity_id` (UUID)
- `embedding` (Vector)
- `model` (String)
- `created_at` (Timestamp)
- Index: (entity_type, entity_id)

#### feature_flags
- `id` (UUID, Primary Key)
- `name` (String, Unique)
- `enabled` (Boolean)
- `description` (Text)
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

#### settings
- `id` (UUID, Primary Key)
- `key` (String, Unique)
- `value` (JSONB)
- `updated_at` (Timestamp)

### Indexes
- B-tree indexes on all foreign keys
- GiST indexes on vector columns (embeddings)
- GIN indexes on JSONB columns (settings)
- Full-text indexes on content fields (posts, profiles)

---

## API

### Authentication Endpoints

#### POST /api/auth/siwe
- **Purpose**: Sign-In with Ethereum
- **Input**: { message, signature }
- **Output**: { token, user }
- **Authentication**: None (public)

#### POST /api/auth/farcaster
- **Purpose**: Farcaster Sign-In
- **Input**: { fid, signature }
- **Output**: { token, profile }
- **Authentication**: None (public)

#### GET /api/auth/session
- **Purpose**: Verify current session
- **Output**: { user, profile }
- **Authentication**: Bearer token

### Profile Endpoints

#### GET /api/profiles/:username
- **Purpose**: Get public profile
- **Output**: Profile data with stats
- **Authentication**: Optional

#### POST /api/profiles/claim
- **Purpose**: Claim a profile
- **Input**: { fid, signature }
- **Output**: { profile, claim }
- **Authentication**: Required

#### PATCH /api/profiles/:username
- **Purpose**: Update claimed profile
- **Input**: { bio, avatar_url, ... }
- **Output**: Updated profile
- **Authentication**: Required (must own profile)

### Post Endpoints

#### GET /api/posts
- **Purpose**: Get timeline/feed
- **Query**: ?cursor=, ?limit=, ?filter=
- **Output**: Paginated posts
- **Authentication**: Optional

#### GET /api/posts/:id
- **Purpose**: Get specific post
- **Output**: Post with replies
- **Authentication**: Optional

#### POST /api/posts/:id/like
- **Purpose**: Like a post
- **Output**: { liked: true }
- **Authentication**: Required

#### DELETE /api/posts/:id/like
- **Purpose**: Unlike a post
- **Output**: { liked: false }
- **Authentication**: Required

#### POST /api/posts/:id/save
- **Purpose**: Save a post
- **Output**: { saved: true }
- **Authentication**: Required

#### DELETE /api/posts/:id/save
- **Purpose**: Unsave a previously saved post
- **Output**: { saved: false }
- **Authentication**: Required

### Social Graph Endpoints

#### POST /api/profiles/:username/follow
- **Purpose**: Follow a profile
- **Output**: { following: true }
- **Authentication**: Required

#### DELETE /api/profiles/:username/follow
- **Purpose**: Unfollow a profile
- **Output**: { following: false }
- **Authentication**: Required

#### GET /api/profiles/:username/followers
- **Purpose**: Get profile's followers
- **Query**: ?cursor=, ?limit=
- **Output**: Paginated profiles
- **Authentication**: Optional

#### GET /api/profiles/:username/following
- **Purpose**: Get profiles followed by user
- **Query**: ?cursor=, ?limit=
- **Output**: Paginated profiles
- **Authentication**: Optional

### Search Endpoints

#### GET /api/search
- **Purpose**: Search posts and profiles
- **Query**: ?q=, ?type=, ?limit=
- **Output**: Search results with ranking
- **Authentication**: Optional

### Admin Endpoints

#### GET /api/admin/workers
- **Purpose**: Get worker health status
- **Output**: Worker statuses
- **Authentication**: Admin token

#### POST /api/admin/flags
- **Purpose**: Update feature flag
- **Input**: { name, enabled }
- **Output**: Updated flag
- **Authentication**: Admin token

#### POST /api/admin/sync/:source/toggle
- **Purpose**: Enable/disable sync source
- **Input**: { enabled }
- **Output**: Updated setting
- **Authentication**: Admin token

### API Standards
- **Authentication**: JWT Bearer tokens
- **Rate Limiting**: Per-user and per-endpoint limits
- **Pagination**: Cursor-based pagination
- **Error Format**: Standardized JSON error responses
- **CORS**: Configurable allowed origins

---

## Rendering

### Server-Side Rendering (SSR)

#### Astro SSR Configuration
- Dynamic route handling for profiles and posts
- On-demand rendering for fresh content
- Partial hydration for interactive components
- Edge-compatible rendering

### SEO Optimization

#### Meta Tags
- Open Graph tags for social sharing
- Twitter Card metadata
- Structured data (JSON-LD)
- Canonical URLs
- Dynamic title and description generation

#### URL Structure
- Clean, semantic URLs
- Profile URLs: `/profile/:username`
- Post URLs: `/post/:id`
- Timeline URLs: `/timeline/:filter`
- Sitemap generation

#### Performance
- **Zero-JS Default**: Base pages work without JavaScript
- **Progressive Enhancement**: JavaScript for interactivity
- **Image Optimization**: Automatic image resizing and WebP conversion
- **Code Splitting**: Component-level code splitting
- **Cache Headers**: Aggressive caching for static content
- **CDN Integration**: Edge caching support

#### Accessibility
- Semantic HTML5 elements
- ARIA labels where appropriate
- Keyboard navigation support
- Screen reader compatibility
- High contrast mode support

### Static Site Generation (SSG)
- Pre-render high-traffic pages
- Incremental static regeneration
- Build-time optimization
- Static asset generation

---

## SmartBrain

SmartBrain is the AI engine powering intelligent features across SocialAi.

### Core Capabilities

#### 1. Text Embeddings
- **Model**: Sentence transformers or OpenAI embeddings
- **Purpose**: Convert text to vector representations
- **Use Cases**:
  - Semantic search
  - Content similarity
  - Topic clustering
  - Recommendation engine

#### 2. Vector Search
- **Storage**: PostgreSQL with pgvector extension
- **Algorithms**: Approximate nearest neighbor (ANN) using HNSW (Hierarchical Navigable Small World) or IVFFlat
- **Features**:
  - Fast similarity search
  - Multi-dimensional indexing
  - Cosine similarity ranking

#### 3. Timeline Summaries
- **Input**: Collection of posts from timeline
- **Processing**: Extractive or abstractive summarization
- **Output**: Concise timeline summary
- **Features**:
  - Key topic extraction
  - Trend identification
  - Daily/weekly summaries

#### 4. Profile Optimization
- **Analysis**: Bio, post history, engagement metrics
- **Suggestions**:
  - Bio improvements
  - Content strategy recommendations
  - Optimal posting times
  - Engagement opportunities

#### 5. Topic Clustering
- **Method**: Unsupervised clustering (K-means, DBSCAN)
- **Input**: Post embeddings
- **Output**: Topic groups with labels
- **Features**:
  - Trending topic detection
  - Community interest mapping
  - Content categorization

#### 6. Recommendations
- **Types**:
  - Content recommendations (posts to read)
  - Profile recommendations (users to follow)
  - Topic recommendations (interests to explore)
- **Algorithms**:
  - Collaborative filtering
  - Content-based filtering
  - Hybrid approach
- **Factors**:
  - User interests
  - Social graph
  - Engagement history
  - Content similarity

### AI Worker Pipeline
1. **Ingestion**: Receive new content from sync workers
2. **Processing**: Generate embeddings and analyze content
3. **Storage**: Save embeddings and metadata to database
4. **Indexing**: Update vector search indexes
5. **Computation**: Calculate recommendations and summaries
6. **Caching**: Cache computed results for quick access

### Model Management
- **Model Selection**: Configurable AI models
- **Version Control**: Track model versions
- **A/B Testing**: Compare model performance
- **Fallback Strategy**: Graceful degradation if AI unavailable

---

## Healdec Engine

Healdec is the **auto-healing, parallel execution engine** that powers SocialAi's backend architecture.

### Architecture Principles

#### 1. One-File Node
- **Design**: Single executable node containing all workers
- **Benefits**:
  - Simplified deployment
  - No microservice complexity
  - Shared memory and state
  - Faster inter-worker communication

#### 2. Parallel Workers
- **Execution**: Workers run in parallel threads/processes
- **Isolation**: Each worker has isolated context
- **Communication**: Inter-worker message passing
- **Coordination**: Shared coordinator for orchestration

#### 3. Auto-Healing
- **Health Checks**: Continuous worker health monitoring
- **Failure Detection**: Automatic crash detection
- **Recovery**: Automatic worker restart
- **State Recovery**: Restore worker state from checkpoints
- **Circuit Breaker**: Prevent cascading failures

### Core Components

#### Health Monitor
- **Heartbeat**: Each worker sends periodic heartbeats
- **Metrics**: CPU, memory, response time tracking
- **Alerts**: Notify on worker degradation
- **Dashboard**: Real-time health visualization

#### Worker Manager
- **Lifecycle**: Start, stop, restart workers
- **Scaling**: Dynamic worker scaling based on load
- **Priority**: Worker priority management
- **Dependencies**: Handle worker dependencies

#### State Manager
- **Checkpoints**: Regular state snapshots
- **Persistence**: Save state to disk/database
- **Recovery**: Restore state after restart
- **Consistency**: Ensure data consistency

#### Event Bus
- **Pub/Sub**: Event-driven architecture
- **Queue**: Message queue for work items
- **Ordering**: Maintain event ordering where needed
- **Reliability**: At-least-once delivery guarantee

### Failure Handling

#### Worker Crash
1. Detect worker has stopped responding
2. Log crash details and stack trace
3. Notify monitoring system
4. Restore last checkpoint
5. Restart worker with recovered state
6. Resume processing

#### Database Connection Loss
1. Detect connection failure
2. Pause workers requiring database
3. Implement exponential backoff retry
4. Restore connection
5. Resume normal operation

#### External API Failure
1. Detect API timeout or error
2. Activate circuit breaker
3. Queue failed requests for retry
4. Fall back to cached data if available
5. Monitor API status
6. Resume when API recovers

### Configuration
- **Worker Count**: Configurable worker instances
- **Recovery Policy**: Restart limits and backoff strategy
- **Resource Limits**: CPU and memory constraints per worker
- **Logging**: Structured logging with log levels
- **Monitoring Integration**: Prometheus, Grafana support

---

## Security

### Authentication & Authorization

#### Sign-In with Ethereum (SIWE)
- **Standard**: EIP-4361 compliant
- **Process**:
  1. User connects wallet
  2. Backend generates nonce
  3. User signs message with nonce
  4. Backend verifies signature
  5. Issue JWT token
- **Security**: Prevents replay attacks with nonce

#### Farcaster Sign-In
- **Method**: Farcaster protocol authentication
- **Verification**: Verify FID and signature
- **Integration**: Direct hub verification
- **Trust**: Cryptographic proof of identity

#### JWT Tokens
- **Algorithm**: RS256 (RSA with SHA-256)
- **Expiration**: Short-lived access tokens (1 hour)
- **Refresh**: Refresh tokens for session extension
- **Storage**: HttpOnly, Secure, SameSite cookies
- **Validation**: Signature verification on each request

#### Authorization Levels
- **Public**: No authentication required
- **Authenticated**: Valid JWT token required
- **Profile Owner**: Must own the profile being modified
- **Admin**: Special admin privileges required

### Data Protection

#### Input Validation
- **Sanitization**: HTML sanitization for user content
- **Length Limits**: Enforce maximum field lengths
- **Type Checking**: Validate data types
- **Format Validation**: Email, URL, ENS format validation
- **SQL Injection Prevention**: Parameterized queries only

#### Output Encoding
- **HTML Encoding**: Escape HTML special characters
- **JSON Encoding**: Proper JSON escaping
- **URL Encoding**: Encode URL parameters
- **XSS Prevention**: Content Security Policy (CSP) headers

#### Rate Limiting
- **Per-User Limits**: Prevent abuse by authenticated users
- **Per-IP Limits**: Protect against DDoS
- **Endpoint-Specific**: Different limits per endpoint type
- **Implementation**: Redis-backed rate limiting
- **Response**: 429 Too Many Requests with Retry-After header

#### CORS (Cross-Origin Resource Sharing)
- **Configuration**: Whitelist allowed origins
- **Credentials**: Allow credentials only for trusted origins
- **Methods**: Restrict allowed HTTP methods
- **Headers**: Control allowed and exposed headers

### Data Privacy

#### Personal Data Handling
- **Minimal Collection**: Collect only necessary data
- **Consent**: User consent for data collection
- **Transparency**: Clear privacy policy
- **User Rights**: Data access, export, deletion

#### Encryption
- **Transport**: TLS 1.3 for all connections
- **At Rest**: Encrypted database storage
- **Keys**: Secure key management
- **Sensitive Data**: Extra encryption for sensitive fields

#### Audit Logging
- **Events**: Log security-relevant events
- **Details**: User, action, timestamp, result
- **Retention**: Configurable log retention
- **Access**: Restricted admin access to logs

### Vulnerability Prevention

#### SQL Injection
- **Method**: Use parameterized queries exclusively
- **ORM**: Leverage ORM's built-in protection
- **Validation**: Validate all user inputs

#### Cross-Site Scripting (XSS)
- **CSP Headers**: Strict Content Security Policy
- **Sanitization**: HTML sanitization library
- **Output Encoding**: Automatic template escaping

#### Cross-Site Request Forgery (CSRF)
- **Tokens**: CSRF tokens for state-changing operations
- **SameSite Cookies**: Prevent cross-site cookie sending
- **Double Submit**: Double-submit cookie pattern

#### Dependency Security
- **Scanning**: Automated dependency vulnerability scanning
- **Updates**: Regular dependency updates
- **Pinning**: Pin dependency versions
- **Review**: Security review for new dependencies

### Abuse Prevention

#### Spam Detection
- **Content Analysis**: Detect spam patterns
- **Rate of Posting**: Limit post frequency
- **Duplicate Detection**: Prevent duplicate content spam

#### Bot Detection
- **Behavioral Analysis**: Detect bot-like behavior
- **CAPTCHA**: Optional CAPTCHA for suspicious activity
- **Device Fingerprinting**: Track device characteristics

#### Moderation Tools
- **Reporting**: User reporting system
- **Review Queue**: Admin review interface
- **Actions**: Hide, delete, ban capabilities
- **Appeals**: User appeal process

### Incident Response
- **Monitoring**: 24/7 security monitoring
- **Alerts**: Immediate notification of security events
- **Response Plan**: Documented incident response procedures
- **Disclosure**: Responsible disclosure policy
- **Backup**: Regular backups for recovery

---

## Conclusion

This technical specification provides a comprehensive overview of the SocialAi architecture, covering the frameworks, worker system, database design, API structure, rendering strategy, AI capabilities, auto-healing engine, and security measures. The system is designed to be scalable, maintainable, and secure while providing a fast, SEO-optimized experience for discovering and claiming social profiles.
