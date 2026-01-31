# 🤖 Agentic Recruitment Engine

An intelligent, **local-first recruitment system** powered by CrewAI agents and Ollama LLMs. Adaptively analyzes resumes against job descriptions with brutally honest assessments, ranks candidates intelligently, and provides context-aware HR insights. Designed for recruiters who demand transparency over sugar-coating.

## 🎯 Core Philosophy

**Adaptive** because the system intelligently adapts:
- 🔄 To different job descriptions and requirements
- 🎯 To varying candidate profiles (junior, mid, senior)
- 📊 By aligning insights with scores (no contradictions)
- 🧠 By learning from multi-agent consensus (JD Matcher → Insight Generator → HR Assistant)

## 🎯 Key Features

### **Core Capabilities**
- **📄 Single Resume Analysis**: Upload a resume and get brutally honest scoring against a job description
- **💬 Adaptive HR Chat**: Ask follow-up questions with context-aware responses that stay consistent with analysis scores
- **🏆 Batch Candidate Ranking**: Compare 2-5 candidates side-by-side and get ranked recommendations
- **🤖 Multi-Agent Intelligence**: Three specialized CrewAI agents that work together:
  - **JD Matcher Agent**: Analyzes resume against JD with strict, no-sugar-coating compatibility scores
  - **Insight Generator Agent**: Generates recommendations that MUST logically align with the match score
  - **HR Chat Agent**: Answers HR-specific questions only about the analyzed candidate (refuses off-topic queries)

### **Smart Scoring System**
- **Technical Skills Assessment** (0-10): Lists matched skills ✅ vs. missing critical skills ❌
- **Experience Level Evaluation** (0-10): Actual years vs. required years with clear gap analysis
- **Education Fit Assessment** (0-10): Degree/certification alignment (Perfect/Good/Fair/Poor)
- **Overall Compatibility Score** (0-100): Final verdict with 1-5 star ratings
- **Consistent Recommendations**: Recommendation tier MUST match the overall score (enforced by Insight Generator agent)

## 📋 Recommendation Tiers
| Score | Recommendation | Stars | Interpretation |
|-------|---|---|---|
| 0-30 | HARD PASS | ⭐ | Not a good fit; major concerns |
| 31-50 | MAYBE | ⭐⭐ | Significant experience/skill gaps |
| 51-70 | CAUTIOUS YES | ⭐⭐⭐ | Moderate fit; has most requirements |
| 71-85 | RECOMMEND | ⭐⭐⭐⭐ | Strong fit; minor gaps acceptable |
| 86-100 | STRONG RECOMMEND | ⭐⭐⭐⭐⭐ | Excellent fit; meets all requirements |

## 🛠️ Tech Stack

- **AI Framework**: CrewAI (multi-agent orchestration + tool execution)
- **LLM Backend**: Ollama (local, open-source models)
- **UI Framework**: Gradio (web interface with file upload/chat)
- **File Processing**: PyPDF2 (PDFs), python-docx (DOCX), encoding support (TXT)
- **Dependencies**: LangChain Community (LLM wrappers), Pydantic 2.5.0 (validation)

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- [Ollama](https://ollama.com) installed and running (`ollama serve`)
- ~4GB RAM minimum (8GB+ recommended)

### Installation & Setup

```bash
# 1. Install dependencies
pip install gradio PyPDF2 python-docx nest_asyncio requests "pydantic==2.5.0" langchain_community "crewai[tools]"

# 2. Ensure Ollama is running
ollama serve &

# 3. Pull a default model (if not already present)
ollama pull llama3.1:8b-instruct-q4_K_M

# 4. Run the application
python Proj.ipynb  # Or launch from Jupyter/VS Code
```

## 📖 Usage Workflows

### **Workflow 1: Single Resume Analysis**
1. Upload a resume (PDF, DOCX, or TXT)
2. Paste the job description
3. Select a model from the dropdown (Llama 3.1, Mistral, etc.)
4. Click **"🤖 Analyze with CrewAI"**
5. Review the analysis:
   - JD Match Score (0-100) with technical, experience, and education breakdowns
   - Critical Insights with recommendation and risk assessment

**Output**: Detailed analysis stored in memory for follow-up HR Chat

### **Workflow 2: HR Chat & Follow-up Questions**
1. After analyzing a resume, navigate to **"💬 HR Chat Assistant"** tab
2. Ask candidate-specific questions like:
   - "Should we hire this candidate and why?"
   - "What are the main skill gaps?"
   - "What should we focus on in the technical interview?"
   - "Does this person meet the 5-year experience requirement?"
3. Get context-aware responses that align with the analysis score
4. The agent will **refuse off-topic questions** (e.g., "What's the weather?")

**Output**: Contextually accurate HR guidance based on candidate's profile

### **Workflow 3: Batch Ranking (2-5 Candidates)**
1. Upload **2-5 resume files** simultaneously
2. Paste the **same job description** for all candidates
3. Click **"🏆 Rank Candidates"**
4. Get a ranked comparison:
   - Ranked by match score (highest to lowest)
   - Medal indicators (🥇 🥈 🥉)
   - Detailed analysis and insights for each candidate

**Output**: Prioritized hiring recommendations with side-by-side comparison

## 📖 Detailed UI Tabs

### **Tab 1: Resume Analysis**
- Upload resume → Paste JD → Select model → Click analyze
- Displays: JD matching score, technical skills gap, experience analysis, education fit
- Enables: HR Chat tab for follow-up questions

### **Tab 2: HR Chat Assistant**
- Prerequisites: Must analyze a resume first (Tab 1)
- Input: Ask any candidate-related question
- Safety: Refuses off-topic questions automatically
- Output: Context-aware, score-aligned responses

### **Tab 3: Batch Resume Ranking**
- Upload 2-5 resumes + 1 JD
- Outputs two sections:
  - **Summary**: Total processed, success/fail counts
  - **Detailed Rankings**: Ranked candidates with scores, stars, recommendations, and full analysis

## 🎛️ Configuration & Customization

### Environment Variables
```python
# Ollama server location
OLLAMA_BASE = os.environ.get("OLLAMA_BASE", "http://127.0.0.1:11434")

# Default LLM model
OLLAMA_MODEL = os.environ.get("OLLAMA_MODEL", "llama3.1:8b-instruct-q4_K_M")

# Suppress LiteLLM verbose logging
LITELLM_LOG = "ERROR"
```

### Token Limits (Tunable in Notebook)
```python
MAX_INPUT_TOKENS    = 2000   # Max tokens for resume + JD combined
MAX_OUTPUT_TOKENS   = 800    # Max tokens per AI response
MAX_QUESTION_TOKENS = 600    # Max tokens for HR chat questions
MAX_CONTEXT_TOKENS  = 1200   # Max tokens for candidate context in HR chat
```

### Model Selection
```python
MODEL_CHOICES = [
    "llama3.1:8b-instruct-q4_K_M",      # Recommended: fast & accurate
    "mistral:7b-instruct-v0.3-q5_0",    # Alternative: balanced
    "llama3.1:8b-instruct",              # Larger: more accurate
    "mistral:7b-instruct",                # Larger: more accurate
    "gpt-oss:20b",                        # Largest: slowest but best
]
```

**Recommendation**: Start with `llama3.1:8b-instruct-q4_K_M` (quantized for speed)

## ⚙️ How It Works

### **Three-Agent Architecture**

```
STEP 1: JD Matcher Agent
├─ Input: Resume text + Job Description
├─ Process: Strict, brutally honest scoring
└─ Output: Match score (0-100), skills gap, experience analysis

        ↓

STEP 2: Insight Generator Agent
├─ Input: Resume + JD Matcher result
├─ Constraint: Recommendation MUST align with score
│  (e.g., score 25 → HARD PASS, score 75 → RECOMMEND)
├─ Process: Generate logical, consistent insights
└─ Output: Recommendation tier + key risks + interview focus

        ↓

STEP 3: HR Chat Agent (Optional)
├─ Input: Candidate-specific question + combined context
├─ Safety: Rejects off-topic questions
├─ Constraint: Stays consistent with analysis scores
└─ Output: Context-aware, score-aligned HR guidance
```

### **Token Flow & Limits**
- **Resume + JD Input**: Capped at 2000 tokens (auto-truncates with marker)
- **Analysis Output**: 800 token limit per response
- **HR Questions**: 600 token limit per question
- **Context Storage**: 1200 token limit for candidate memory
- **Truncation Strategy**: Keeps first N tokens + "[TRUNCATED: ...]" marker

### **Safety Constraints**
1. **Context Isolation**: Only discusses the currently analyzed candidate
2. **Score Alignment**: Insights must logically match the match score
3. **Input Validation**: Token capping prevents prompt injection
4. **Question Filtering**: Candidate-related keywords enforced
5. **Error Handling**: Graceful fallbacks for API/model failures

## 📁 Supported File Formats & Limits

| Format | Support | Details |
|--------|---------|---------|
| **PDF** | ✅ | Text extraction from pages; encrypted PDFs rejected with error |
| **DOCX** | ✅ | Extracts paragraphs + tables |
| **TXT** | ✅ | Supports UTF-8, Latin-1, CP1252, ISO-8859-1 encodings |
| **Max Size** | 10 MB | Files > 10MB auto-rejected |
| **Empty Files** | ❌ | Rejected; must contain text content |

### **File Processing Details**
- PDF: Page-by-page extraction; skips unreadable pages with warnings
- DOCX: Combines all paragraph text + table cell content
- TXT: Auto-detects encoding; tries multiple formats
- Error Handling: Clear error messages for unsupported formats

## 🔍 Key Algorithms & Logic

### **Score Extraction Algorithm**
Extracts numeric match scores from AI responses using regex patterns with fallbacks:
```python
# Pattern 1: "OVERALL MATCH SCORE: X/100"
# Pattern 2: "MATCH SCORE: X/100"
# Pattern 3: "SCORE: X/100"
# Pattern 4: Fallback "X/100"
# Range: Always clamped to 0-100
```
**Example**: Response with "**OVERALL MATCH SCORE: 72/100**" → Extracts 72

### **Question Relevance Filter (HR Chat)**
Determines if a question is candidate-related or off-topic:
```python
# UNRELATED keywords:
["weather", "recipe", "cooking", "sports", "politics", "movie", "math", ...]

# RELATED keywords:
["candidate", "hire", "skill", "experience", "interview", "score", 
 "qualify", "recommend", "strength", "weakness", "concern", ...]

# Default behavior: If unrelated keywords match → REJECT
# Otherwise: If related keywords match → ACCEPT
# Fallback: Accept by default (permissive)
```

### **Score-Recommendation Alignment (Insight Generator)**
Ensures recommendations logically match scores:
```python
# Score 0-30:   HARD PASS     (mandatory)
# Score 31-50:  MAYBE         (candidate needs to be cautious)
# Score 51-70:  CAUTIOUS YES  (hire with reservations)
# Score 71-85:  RECOMMEND     (good hire)
# Score 86-100: STRONG RECOMMEND (excellent hire)
```
**Enforced by Insight Generator Agent**: If alignment violated, agent regenerates response

## 🧠 CrewAI Agents Architecture

| Component | Role | Input | Process | Output | Tools |
|-----------|------|-------|---------|--------|-------|
| **JD Matcher Agent** | Brutally Honest Job Matching Specialist | Resume + JD | Strict evaluation against requirements | Score (0-100) + skills/experience/education breakdown | `jd_match_tool` |
| **Insight Generator Agent** | Logically Consistent Career Assessor | Resume + JD Result | Align recommendation with score (enforced) | Recommendation + Key Risks + Interview Focus | `critical_insight_tool` |
| **HR Chat Agent** | Focused HR Assistant | Question + Context | Filter off-topic queries; answer candidate-specific questions | HR guidance aligned with scores | `hr_chat_tool` |

### **Agent Constraints**
- **No Delegation**: Each agent works independently (prevents loops)
- **Verbose Off**: Reduces noise; only displays final outputs
- **Tool Integration**: Each agent has exactly one specialized tool
- **LLM Binding**: All agents use the same Ollama model instance

## 🔒 Safety, Privacy & Constraints

### **Data Privacy**
- ✅ **Local Processing Only**: All data stays on your machine; no cloud uploads
- ✅ **No External APIs**: Uses local Ollama instance (no third-party LLM calls)
- ✅ **No Logging**: Candidate data not persisted after analysis session

### **Analysis Constraints**
- ✅ **Context Isolation**: Only discusses the currently analyzed candidate
- ✅ **Score Alignment**: Recommendations must logically match JD scores
- ✅ **Input Validation**: Token capping prevents prompt injection attacks
- ✅ **Question Filtering**: Off-topic question detection prevents misuse
- ✅ **Error Handling**: Graceful failures with user-friendly error messages

### **Model Safety**
- ✅ **Temperature Control**: Set to 0.2 (low) for consistent, deterministic outputs
- ✅ **Token Limits**: Prevents infinite/excessive model responses
- ✅ **Fallback Mechanisms**: If chat endpoint unavailable, falls back to generate endpoint
- ✅ **Dummy API Keys**: OPENAI_API_KEY set to dummy value (doesn't call OpenAI)

## ⚡ Performance Tips

1. **Use Quantized Models**: `q4_K_M` or `q5_0` for speed (vs. full-precision)
2. **Batch Analysis**: Rank multiple candidates in one go
3. **GPU Acceleration**: If available, Ollama uses GPU automatically
4. **Model Selection**: Smaller models (7B) run faster; larger (13B+) more accurate

## 📊 Output Examples

### **Example 1: Resume Analysis Output**
![alt text](images/Ex1.png)

### **Example 2: HR Chat Interaction**
![alt text](images/Ex2.png)

### **Example 3: Batch Ranking Summary**
![alt text](images/Ex3.png)


---
