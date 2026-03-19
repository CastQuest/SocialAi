# 🌐 SocialAi  
### The Open Social Index & Identity Claim Network

SocialAi is a lightweight, AI‑powered social discovery engine that mirrors public Farcaster activity, blends optional Reddit timelines, and exposes SEO‑optimized public profiles that users can claim by verifying their Farcaster identity.

It is built on a **parallel, auto‑healing, one‑file node architecture** powered by Healdec and SmartBrain.

---

## 📋 Master Prompt

See [`PROMPT.md`](./PROMPT.md) for the master prompt used to generate production-ready full-stack applications with this stack. Feed it to any AI coding agent to scaffold a complete, working implementation.

---

## ✨ Features

### 🔍 Public Social Index
- SEO‑optimized profiles, posts, timelines  
- Google‑indexable pages  
- Zero‑JS by default (Astro + Vite)  
- Fast SSR for timelines and profiles  

### 🪪 Identity Claim System
- Farcaster Sign‑In  
- Wallet verification (SIWE)  
- ENS mapping  
- Claim → unlock editing + AI tools  

### 🌐 Multi‑Network Sync
- Farcaster Hub ingestion  
- Optional Reddit sync  
- Future: Lens, Bluesky, Zora  
- Admin‑controlled feed toggles  

### 🤖 SocialAi Brain
- Embeddings + vector search  
- Timeline summaries  
- Profile optimization  
- Topic clustering  
- Recommendations  

### 🧩 Social Graph
- Follow / unfollow  
- Likes  
- Saved posts  
- Mutuals  

### 🛠 Admin Console
- Feature flags  
- Sync toggles  
- Logs  
- Worker health  
- Abuse controls  

---

## 🧱 Architecture

### Public Layer (Astro + Vite)
- SEO pages  
- Profiles  
- Timelines  
- Claim flow  
- Landing pages  

### Admin Layer (Angular)
- Feature flags  
- Sync controls  
- Worker monitoring  
- System health  

### Backend (One‑File SocialAi Node)
- Healdec engine  
- Parallel chain workers  
- AI worker  
- Search worker  
- Sync worker  
- RPC workers  

### Database (Postgres)
- Users  
- Profiles  
- Posts  
- External posts  
- Follows  
- Likes  
- Claims  
- Embeddings  
- Feature flags  
- Settings  

---

## 🚀 Getting Started

```bash
git clone https://github.com/<org>/socialai
cd socialai
npm install
npm run dev
