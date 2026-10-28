# Local-AI-Workloads
 4-week syllabus outline designed for an academic course or intensive technical workshop using Docker for local AI worload 

Course Overview

• Course Title: Containerizing and Securing Local AI Workloads
• Prerequisites: Basic Linux command line, introductory Python, and foundational Docker concepts (images, containers, volumes).
• Target Audience: DevOps, Cloud Engineering, or Applied AI students.

Week 1: Foundations of Local AI & Containerization

• Objective: Understand the architectural shift from cloud-based AI APIs to private, locally containerized AI models.
• Core Lecture Topics:
	• The landscape of local AI vs. SaaS APIs (OpenAI, Anthropic).
	• Data privacy compliance (GDPR/CCPA) and intellectual property protection in AI.
	• System requirements: CPU vs. GPU acceleration, VRAM allocation, and quantization concepts.
• Lab / Hands-on Activity:
	• Setting up the student environment (Docker Desktop / Docker Engine).
	• Pulling and exploring foundational open-weights models (e.g., Llama 3, Mistral) onto local storage.

Week 2: Constructing the AI Container Environment

• Objective: Write optimized Dockerfiles tailored for AI runtime engines and build reusable environments.
• Core Lecture Topics:
	• Architecting Dockerfiles for heavy workloads: caching layers, base image selection (Ubuntu vs. Alpine for data science).
	• Introducing the Docker Model Runner architecture.
	• Managing large model files effectively using Docker volumes and bind mounts.
• Lab / Hands-on Activity:
	• Writing a multi-stage Dockerfile to containerize a local LLM runner.
	• Configuring volume mounts so model weights persist outside the container lifecycle.
	• Optimizing build times using Docker build cache.

Week 3: Securing and Packaging the Workload

• Objective: Implement container security best practices and distribute the packaged AI workload safely.
• Core Lecture Topics:
	• Principle of Least Privilege: Running AI containers as a non-root user.
	• Image scanning and vulnerability assessment (using Docker Scout or Trivy).
	• Container distribution: Tagging strategies and securely pushing to Docker Hub or private registries.
• Lab / Hands-on Activity:
	• Refactoring the Week 2 Dockerfile to enforce user namespace isolation (USER 1001).
	• Running security scans on the built image, identifying vulnerabilities, and applying patches.
	• Authenticating and pushing the final secure image to a container registry.

Week 4: API Integration, Testing, and Deployment

• Objective: Expose the containerized model through a secure, standardized API interface and build a consumer application.
• Core Lecture Topics:
	• The OpenAI-compatible API standard and why it matters for interoperability.
	• Network isolation: Docker bridge networks and port mapping constraints.
	• Troubleshooting and monitoring container logs under heavy inference loads.
• Lab / Hands-on Activity:
	• Deploying the AI runner container alongside a lightweight web application (e.g., Streamlit or a Python script) using Docker networking.
	• Routing user queries through the consumer app to the secure local API endpoint.
	• Final Capstone Presentation: Students demonstrate a fully offline, private AI chat assistant running completely inside isolated containers.


Part 1: Lab Grading Criteria & Rubrics

AI containers are heavy and security configuration errors can easily break them. A holistic 100-point rubric split into four core pillars ensures students focus equally on performance, configuration, and security.

📊 100-Point Lab Rubric Breakdown


Evaluation Pillar	Weight	Target Criteria (What to look for)
1. Functional Execution & Performance	30%	• The LLM runs locally without crashing.
• The API responds to queries accurately.
• Execution speeds match the constraints of the assigned hardware.
2. Configuration & Optimization	30%	• Dockerfile layers are optimized (e.g., correct ordering to utilize layer cache).
• Model weights are properly mapped via Docker volumes or bind mounts (weights should never be baked directly into the image layer).
• Environment variables are used for configurations rather than hardcoded scripts.
3. Security Implementation	25%	• The container runs under a designated non-root user account (e.g., USER 1001).
• Docker Scout / Trivy scan reports show zero Critical or High vulnerabilities that could have been resolved via base image updates.
• No sensitive API tokens or passwords are left exposed in plain text inside the Dockerfile.
4. Documentation & Reproducibility	15%	• The submitted repository contains a clear README.md.
• The commands required to build, run, and query the container are explicitly documented and repeatable by the instructor.


Part 2: Student Lab Hardware Recommendations

Running LLMs locally requires significantly more resources than traditional web-development containers. Below are the minimum and recommended hardware tiers for student machines.

🖥️ Hardware Tiers


Component	Minimum Specification (CPU-Only / Small Models)	Recommended Specification (GPU-Accelerated / Standard Models)
Target Models	1B–3B parameter models (e.g., Llama-3-8B-Instruct-Q4_K_M)	7B–8B parameter models (e.g., Llama 3 8B, Mistral 7B)
Processor (CPU)	Intel Core i5 / AMD Ryzen 5 (6+ Cores)	Intel Core i7 / AMD Ryzen 7 or Apple Silicon (M1/M2/M3 Pro/Max)
System RAM	16 GB (Crucial, as the OS and Docker Desktop consume significant overhead)	32 GB (Highly recommended for smooth multi-container setups)
Graphics (GPU)	Integrated Graphics (Relies on CPU-bound AVX2 matrix math)	Dedicated NVIDIA GPU with 8GB+ VRAM (e.g., RTX 3060/4060) to leverage CUDA acceleration inside Docker
Storage	50 GB free space on a fast NVMe SSD	100 GB+ free space on a fast NVMe SSD (Model weights take 4GB–10GB each)


💡 Instructor Tips for Hardware Constraints

If your academy's current lab infrastructure falls under the Minimum Spec tier, you can save your classes from system freezes by enforcing these rules:
1. Mandate Quantized Models: Require students to exclusively pull 4-bit quantized models (usually labeled as .Q4_K_M or GGUF format). These compress an 8B model from ~16GB down to roughly 4.8GB, allowing them to comfortably sit in system RAM.
2. Configure Docker Resource Limits: Teach students to modify their Docker Desktop settings to cap RAM allocation (e.g., limit Docker to 8GB out of 16GB available) so the host operating system doesn't crash from out-of-memory (OOM) errors.

