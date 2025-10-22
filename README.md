import json
import os
import datetime

FILE = "pro_prompts.json"

# ----------------------- DATA -----------------------
def load_prompts():
    if os.path.exists(FILE):
        with open(FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_prompts(data):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=4, ensure_ascii=False)

# ----------------------- CREATE PROMPT -----------------------
def create_prompt(prompts):
    print("\n🧠 Create a Professional Prompt\n")

    role = input("🎭 Role for AI (e.g., expert designer, senior developer, marketing strategist): ").strip()
    goal = input("🎯 Goal (What should AI achieve?): ").strip()
    style = input("🎨 Writing Style (e.g., formal, creative, informative): ").strip()
    tone = input("🎵 Tone (e.g., friendly, academic, persuasive): ").strip()
    audience = input("👥 Target Audience (optional): ").strip()
    output_format = input("📦 Output Format (e.g., bullet points, table, code, paragraph): ").strip()
    keywords = input("🔑 Key Concepts or Focus Words (comma separated): ").strip().split(",")
    constraints = input("🚫 Constraints or Rules (optional): ").strip()

    prompt_text = f"""
You are {role}.
Your task: {goal}.
Writing style: {style}.
Tone: {tone}.
Audience: {audience if audience else "general"}.
Output format: {output_format}.
Focus on: {', '.join([k.strip() for k in keywords if k.strip()])}.
{f"Constraints: {constraints}" if constraints else ""}
Be clear, structured, and result-oriented.
Respond only with the requested content.
    """.strip()

    print("\n✨ Generated Prompt:\n")
    print(prompt_text)

    prompts.append({
        "role": role,
        "goal": goal,
        "style": style,
        "tone": tone,
        "audience": audience,
        "format": output_format,
        "keywords": keywords,
        "constraints": constraints,
        "prompt": prompt_text,
        "created_at": datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
    })

    save_prompts(prompts)
    print("\n✅ Prompt saved successfully!")

# ----------------------- VIEW & SEARCH -----------------------
def view_prompts(prompts):
    if not prompts:
        print("\n📭 No saved prompts yet.")
        return

    print("\n=== 💾 SAVED PROMPTS ===")
    for i, p in enumerate(prompts, 1):
        print(f"{i}. {p['role']} → {p['goal']} ({p['style']}, {p['tone']})")

def search_prompts(prompts):
    keyword = input("🔍 Search by role, goal, or keyword: ").strip().lower()
    results = [
        p for p in prompts
        if keyword in p["role"].lower()
        or keyword in p["goal"].lower()
        or any(keyword in k.lower() for k in p["keywords"])
    ]

    if not results:
        print("❌ No matching prompts found.")
        return

    print(f"\n📘 Found {len(results)} prompt(s):\n")
    for r in results:
        print(f"🎭 Role: {r['role']}")
        print(f"🎯 Goal: {r['goal']}")
        print(f"🗓️ Created: {r['created_at']}")
        print(f"📝 Prompt:\n{r['prompt']}")
        print("-" * 60)

# ----------------------- MAIN -----------------------
def main():
    prompts = load_prompts()
    while True:
        print("\n=== 🤖 PROMPT ENGINEERING ASSISTANT ===")
        print("1️⃣ Create new prompt")
        print("2️⃣ View saved prompts")
        print("3️⃣ Search prompts")
        print("4️⃣ Exit")

        choice = input("👉 Choose an option: ").strip()
        if choice == "1":
            create_prompt(prompts)
        elif choice == "2":
            view_prompts(prompts)
        elif choice == "3":
            search_prompts(prompts)
        elif choice == "4":
            print("👋 Stay sharp, Prompt Engineer!")
            break
        else:
            print("⚠️ Invalid choice, try again.")

if __name__ == "__main__":
    main()

