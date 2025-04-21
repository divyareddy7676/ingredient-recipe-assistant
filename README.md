# 🧺 Ingredient-Based Recipe Assistant

🎓 *Capstone Project – Gen AI Intensive Course (2025Q1)*  
🔗 Built with: **LangGraph**, **Google Gemini API**, **FAISS**, **LangChain**

---

## 🧠 Overview

This project is a smart **recipe generator** that creates nutritious meal suggestions based on a list of ingredients provided by the user.

You can input:  
> `"I want a Mexican dinner with rice and beef"`  
or just:  
> `"rice, beef, corn"`

And the assistant will return:
- 🍽️ A structured recipe suggestion
- 📋 Ingredient list
- 📝 Cooking steps
- 🧠 Evaluation on ease & nutrition

---

## 🧰 Technologies Used

| Tool | Purpose |
|------|---------|
| [Google Gemini API](https://ai.google.dev/) | Recipe generation & evaluation |
| [LangGraph](https://www.langchain.com/langgraph) | Agent flow controller |
| [FAISS](https://github.com/facebookresearch/faiss) | Vector search for recipe memory |
| [LangChain](https://www.langchain.com/) | Language model interface layer |
| Python | Orchestration and app logic |
| Kaggle | Notebook hosting and experimentation |

---

## ⚙️ How It Works

### 💡 System Flow

```text
Start
  ↓
User Input or Static Prompt
  ↓
LangGraph Agent
  ├─> Gemini Prompt → Recipe Suggestion
  ├─> Gemini Prompt → Evaluation
  ↓
Final Output
```

✅ Each stage runs automatically on "Run All" — no interaction needed!

---

## 📌 Key Components

### 🔁 LangGraph Agent

LangGraph orchestrates interaction between:
- `user_input_node()` (static or user-provided input)
- `chatbot_node()` (handles Gemini prompts)
- `router()` (determines when to stop)

```python
graph = StateGraph(RecipeState)
graph.add_node("chatbot", chatbot_node)
graph.add_node("user_input", user_input_node)
graph.set_entry_point("user_input")
graph.add_conditional_edges("user_input", router)
graph.add_edge("chatbot", "user_input")
recipe_agent = graph.compile()
```

---

### 📝 Gemini Prompt (Recipe)

```python
prompt = f'''
You are a smart cooking assistant.

The user gave this request:
"{user_input}"

Extract:
- Ingredients
- Cuisine
- Meal Type
- Flavor

Suggest one recipe in this format:
- Recipe Summary
- Ingredients
- Instructions
'''
```

---

### 🧠 Gemini Evaluation

The model rates:
- **Ease of preparation**
- **Nutrition value**

```python
eval_prompt = f'''
Evaluate the following recipe based on:
- Ease of preparation
- Nutrition

Recipe:
{recipe_text}
'''
```

---

### 🧲 RAG (Vector Search)

We use FAISS to retrieve recipes similar to a query and re-contextualize them:

```python
results = vector_db.similarity_search(query, k=2)
context = "\n".join(doc.page_content for doc in results)
```

Then Gemini generates a new recipe using the retrieved results.

---

## 🌟 Advantages

✅ Works with minimal input  
✅ Easy to run in "Run All" mode  
✅ Highly modular and reusable (swap model or add UI)  
✅ Handles both generation and evaluation  
✅ Retrieves similar recipes using RAG

---

## 🚀 Future Scope

Here’s how this project could evolve further:

### 📈 1. Fine-tuned or multi-modal models
- Integrate **Gemini Vision** to process fridge or grocery images

### 🔊 2. Voice-based input
- Use `SpeechRecognition` + `PyAudio` for hands-free cooking

### 📋 3. Meal Planning
- Extend the flow to generate **weekly plans** or **shopping lists**

### 📊 4. Feedback Loop (Day 5 MLOps)
- Log feedback and improve suggestion ranking over time

### 🍱 5. Dietary Tagging
- Add tags like `"vegan"`, `"gluten-free"`, `"low-carb"` using Gemini classification

---

## ✅ Run It Yourself

1. Clone the repo:
```bash
git clone https://github.com/your-username/ingredient-recipe-assistant.git
cd ingredient-recipe-assistant
```

2. Open the notebook or run it in [Kaggle](https://www.kaggle.com/)

---

## 📜 License

Apache 2.0  
(c) 2025 Your Name / Gen AI Capstone
