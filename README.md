# CoMark: Real-Time Collaborative Canvas

> A high-performance, multi-user whiteboard built for seamless brainstorming. Featuring real-time state synchronization, persistent rooms, and live cursor tracking.

---

## The Mission
Most collaborative tools suffer from high latency or complex setup. **CoMark** was built to solve the "Real-Time Problem"—exploring how to maintain a perfectly synced state between multiple users across the globe with sub-50ms latency. 

By leveraging **PostgreSQL** and **WebSockets**, this project demonstrates a scalable approach to distributed state management in a serverless environment.

## The Tech Stack

| Layer | Technology | Why? |
| :--- | :--- | :--- |
| **Frontend** | React + Next.js | Chosen for efficient component-based UI and high-speed page loads. |
| **Styling** | Tailwind CSS | Used for a lightweight, utility-first design system. |
| **Real-time/DB** | **Supabase** | Utilizes Postgres CDC (Change Data Capture) for enterprise-grade data syncing. |
| **Canvas Engine** | tldraw | A powerful vector engine that handles complex canvas math and rendering. |
| **Deployment** | Vercel | Global edge deployment to ensure the lowest possible latency for users worldwide. |

---