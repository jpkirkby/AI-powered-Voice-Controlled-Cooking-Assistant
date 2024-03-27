# AI-powered-Voice-Controlled-Cooking-Assistant
开发一款AI驱动的语音控制烹饪助手，根据用户的口味和饮食限制提供个性化食谱和指导。
import speech_recognition as sr
from gtts import gTTS
import os
import random

# This is a placeholder for the recipe database
# In a real application, this would likely be a more complex database or API
recipes = {
    "vegetarian": ["Vegetable Stir Fry", "Caprese Salad"],
    "vegan": ["Vegan Lasagna", "Vegan Chocolate Cake"],
    "gluten-free": ["Gluten-Free Chicken Parmesan", "Gluten-Free Pancakes"],
}

def listen_for_command():
    """
    Listens for a voice command from the user.
    """
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening for your cooking preferences...")
        audio = r.listen(source)
    
    try:
        command = r.recognize_google(audio).lower()
        return command
    except sr.UnknownValueError:
        return "I couldn't understand you. Please try again."
    except sr.RequestError:
        return "Could not request results; check your internet connection."

def find_recipe(diet_preference):
    """
    Finds a recipe based on the user's dietary preference.
    """
    if diet_preference in recipes:
        return random.choice(recipes[diet_preference])
    else:
        return "No recipes found for your dietary preference."

def speak(text):
    """
    Uses Google TTS to speak out the guidance or recipe.
    """
    tts = gTTS(text=text, lang='en')
    tts.save("output.mp3")
    os.system("start output.mp3")

def main():
    print("Hello! I'm your cooking assistant.")
    speak("Hello! I'm your cooking assistant. What's your dietary preference?")
    
    # This is simplified; real-world application would involve more complex logic
    command = listen_for_command()
    print(f"You said: {command}")
    
    recipe = find_recipe(command)
    print(recipe)
    speak(f"How about we make {recipe} today?")

if __name__ == "__main__":
    main()
