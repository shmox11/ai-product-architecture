# Living Resume - Technical Architecture

## Problem & Product Vision

**Problem**: Traditional resumes fail to showcase technical depth and real-time capabilities in AI/ML roles. Static PDFs can't demonstrate coding skills, project complexity, or ongoing learning.

**Product Vision**: An interactive "living resume" that combines portfolio showcase, technical documentation, and AI-powered Q&A to provide a comprehensive view of technical capabilities and product thinking.

**Success Metrics**: 
- Engagement depth (time on site, interactions)
- Conversion to meaningful conversations/interviews
- Demonstration of full-stack AI development skills

## System Architecture

```
┌─────────────────┐    ┌───────────────────┐    ┌─────────────────┐
│   React Frontend │    │  FastAPI Backend  │    │  PostgreSQL DB │
│                 │    │                   │    │                 │
│ • Portfolio UI  │◄──►│ • Chat API        │◄──►│ • Chat History  │
│ • Chat Interface│    │ • Document Search │    │ • User Sessions │
│ • Analytics     │    │ • OpenAI Proxy    │    │ • Analytics     │
└─────────────────┘    └───────────────────┘    └─────────────────┘
         │                       │                       
         │              ┌───────────────────┐            
         │              │   OpenAI API      │            
         │              │                   │            
         └──────────────┤ • GPT-4 Chat      │            
                        │ • Context Window  │            
                        │ • System Prompts  │            
                        └───────────────────┘            

Infrastructure: Google Cloud Run + Cloud SQL + Artifact Registry
CI/CD: GitHub Actions → Docker → Cloud Run
```

## Technical Product Decisions

### Frontend Architecture
**Decision**: React SPA with Framer Motion animations
**Rationale**: Needed smooth UX for portfolio browsing and real-time chat. Framer Motion provides professional animations without performance overhead.
**Tradeoff**: Client-side rendering vs SEO - mitigated with proper meta tags and noscript fallbacks.

### Backend Architecture  
**Decision**: FastAPI with async/await patterns
**Rationale**: Python ecosystem for AI integration, async for chat responsiveness, automatic OpenAPI docs for development.
**Tradeoff**: Python vs Node.js - chose Python for better AI/ML library ecosystem and team familiarity.

### Database Design
**Decision**: PostgreSQL with simple schema (chat_sessions, messages)
**Rationale**: ACID compliance for chat history, JSON column support for flexible message metadata, Cloud SQL managed service.
**Tradeoff**: SQL vs NoSQL - chose SQL for data consistency and complex query capabilities.

### AI Integration
**Decision**: OpenAI GPT-4 with RAG (Retrieval Augmented Generation)
**Rationale**: High-quality responses, good context window, reliable API. RAG ensures answers stay grounded in actual project data.
**Tradeoff**: Cost vs quality - GPT-4 more expensive but significantly better for professional use case.

### Deployment Strategy
**Decision**: Containerized deployment on Google Cloud Run
**Rationale**: Auto-scaling, pay-per-use pricing, easy CI/CD integration, regional deployment options.
**Tradeoff**: Platform lock-in vs managed services - chose managed for faster iteration.

## Implementation Highlights

### 1. AI Chat System
**Challenge**: Keep responses accurate to actual experience without hallucination
**Solution**: 
- Curated document store with project facts, technical details, and learning journey
- System prompts emphasizing authentic positioning ("AI Product Engineer transitioning to PM")
- Context injection with source attribution

### 2. Real-time Chat UX
**Challenge**: Professional chat experience with low latency
**Solution**:
- WebSocket-like streaming via Server-Sent Events
- Optimistic UI updates with error handling
- Prompt chips for guided interaction
- Analytics tracking for conversation quality

### 3. Portfolio Architecture
**Challenge**: Showcase technical projects without exposing private code
**Solution**:
- Project metadata schema with technical artifacts
- Mini-schemas showing "What I built / Tech stack / Challenges / Learning"
- Links to architecture docs, demos, and public repos only

### 4. Performance & Scalability
**Challenge**: Fast loading for portfolio browsing, responsive chat
**Solution**:
- Code splitting and lazy loading for React components
- PostgreSQL connection pooling and query optimization
- Cloud Run auto-scaling based on request volume
- CDN for static assets (images, documents)

### 5. Security & Privacy
**Challenge**: Public-facing app with database and API integration
**Solution**:
- CORS configuration for allowed origins
- Rate limiting on chat API endpoints
- No PII storage (anonymous chat sessions)
- Environment-based secrets management

## Technical Stack Rationale

| Layer | Technology | Why This Choice |
|-------|------------|-----------------|
| Frontend | React + Vite | Component reusability, fast dev server, modern tooling |
| Styling | Tailwind CSS | Rapid prototyping, consistent design system, small bundle |
| Animations | Framer Motion | Smooth portfolio transitions, professional polish |
| Backend | FastAPI | Async Python, automatic docs, type hints, fast development |
| Database | PostgreSQL | ACID compliance, JSON support, mature ecosystem |
| AI | OpenAI GPT-4 | Best-in-class responses, reliable API, good documentation |
| Infrastructure | Google Cloud | Managed services, auto-scaling, integrated CI/CD |
| CI/CD | GitHub Actions | Free for public repos, Docker integration, familiar workflow |

## Results & Learning

### What Worked Well
- **Authentic Positioning**: Focusing on "AI Product Engineer → PM" resonated better than inflated claims
- **LLM-Assisted Development**: Significantly faster iteration cycles without sacrificing code quality
- **Modern Stack**: React + FastAPI + PostgreSQL proved reliable and scalable
- **Cloud-Native**: Auto-scaling and managed services reduced operational overhead

### Technical Challenges Overcome
- **CORS Configuration**: Dynamic frontend URL handling in CI/CD pipeline
- **Database Connection**: Cloud SQL socket format and connection pooling
- **Chat Context Management**: Balancing response quality with API cost optimization
- **Deployment Automation**: End-to-end CI/CD with secret management

### What I'd Do Differently
- **Frontend Architecture**: Consider Next.js for better SEO and static generation
- **Database Design**: Add proper user authentication and session management
- **AI Integration**: Implement vector embeddings for better document retrieval
- **Analytics**: Add more detailed conversation flow tracking

### Next Iteration Plans
- **Enhanced RAG**: Vector database for semantic document search
- **User Profiles**: Optional accounts for personalized conversation history
- **Multi-Modal**: Support for image uploads and visual project descriptions
- **Performance**: Implement edge caching and further optimize bundle size

## Key Learnings for Product Engineering
1. **Technical Decisions Impact UX**: Database choice affects chat responsiveness
2. **Deployment Strategy Enables Iteration**: Cloud-native approach accelerated feature development
3. **AI Integration Requires Product Thinking**: Balancing accuracy, cost, and user experience
4. **Authentic Positioning Beats Inflation**: Technical depth resonates more than inflated metrics
5. **Documentation as Product**: Architecture docs demonstrate technical communication skills

---

*This living resume demonstrates full-stack AI application development with modern tooling, cloud deployment, and product-focused technical decision making.*