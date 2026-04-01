
### 3. `architecture.md`
```markdown
# Private Cloud Architecture

The private cloud was designed to simulate a real OpenStack environment using a Ubuntu 22.04 KVM VM as the compute node.

**High-Level Flow:**  
User (internal) → Browser → Private Cloud VM → Docker Container → Flask App + Persistent Volume

All traffic remains on the internal tenant network (192.168.100.0/24). No public exposure. Persistent storage uses a Docker volume (Cinder equivalent).

(This matches the architecture diagram in the final report.)