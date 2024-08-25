# AI and GPT Bootcamp: Homework for Week 1

## Introduction

In this repository, you will find the code and documentation for the Week 1 homework of the Encode Club AI and GPT Bootcamp Q3 2024. The main goal of this assignment is to use the OpenAI API to create an Indian chef assistant specializing in vegetarian cuisine, with a focus on making paneer pulao.

## Exercise Statement

The exercise is based on the Weekend Project section at the end of Lesson 4. The base code can be found in IndianChefGPT.py.

For detailed instructions, please refer to: [Chef GPT Instructions](https://github.com/Encode-Club-AI-Bootcamp/Generative-AI-Applications/blob/main/Lesson-04/exercises/07-Chef-GPT.md)

## Jupyter Notebook

The complete Python code is available in this Jupyter notebook: [Encode Club AI and GPT Bootcamp Q3 Homework for Week 1](https://github.com/LuisJoseSanchez/encode-club-ai-and-gpt-bootcamp-q3-homework-week1/blob/master/Encode_Club_AI_and_GPT_Bootcamp_Q3_Homework_for_week_1.ipynb)

Note: You will need to provide your own API key to run the code.

## Python Code Explained

### Installing the OpenAI Package

First, we install the OpenAI package. In Google Colab, we use the following command:

```python
# Install openai API package
!pip install openai
```

The output will look similar to this:

```
Collecting openai
  Downloading openai-1.42.0-py3-none-any.whl.metadata (22 kB)
Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.10/dist-packages (from openai) (3.7.1)
Requirement already satisfied: distro<2,>=1.7.0 in /usr/lib/python3/dist-packages (from openai) (1.7.0)
Collecting httpx<1,>=0.23.0 (from openai)
...
Installing collected packages: jiter, h11, httpcore, httpx, openai
Successfully installed h11-0.14.0 httpcore-1.0.5 httpx-0.27.0 jiter-0.5.0 openai-1.42.0
```

### Importing and Configuring OpenAI

We import the OpenAI class and create a client instance:

```python
# Import the openai module
from openai import OpenAI
# Create a new client instance
client = OpenAI(api_key="YOUR_API_KEY_HERE")
```

Make sure to replace "YOUR_API_KEY_HERE" with your actual OpenAI API key.

### Configuring the AI Assistant

We set up the AI to act as an Indian chef specializing in vegetarian cuisine:

```python
# Set the first message
messages = [
  {
    "role": "system",
    "content": "You are an experienced Indian chef specializing in vegetarian \
                cuisine. You are willing to help people by suggesting \
                detailed vegetarian recipes for Indian dishes they want to cook. \
                If someone asks you about non-vegetarian dishes, politely tell them \
                that you only handle inquiries about vegetarian Indian cuisine.",
  }
]
# Add another system instruction
messages.append(
  {
    "role": "system",
    "content": "The client will ask you one of these three topics: \
                ingredient-based dish suggestions, recipe requests for \
                specific dishes or recipe critiques and improvement \
                suggestions. If you don't know the dish or the \
                ingredient, you should answer that you don't know the \
                dish or ingredient and end the conversation.",
  }
)
# More instructions
messages.append(
  {
    "role": "system",
    "content": "If the client tells you an ingredient, suggest only dish \
                names without full recipes. If you are asked for a dish \
                name then provide a detailed recipe. If the client tells \
                you a recipe, offer suggested improvements.",
  }
)
```

### Selecting the AI Model

We specify the model to use:

```python
# Specify the model used
model = "gpt-4-mini"
```

### User Interaction

We present a menu for the user to choose their desired action:

```python
# Menu
while True:
  print("1. I want the recipe for a specific Indian dish")
  print("2. I want a recipe for a specific Indian ingredient")
  print("3. I want suggestions to improve an Indian recipe that I know")
  option = input("Choose an option (1-3): ")

  if option in ["1", "2", "3"]:
    break
  else:
    print("Invalid option. Please choose 1, 2, or 3.")
```

Example output:
```
1. I want the recipe for a specific Indian dish
2. I want a recipe for a specific Indian ingredient
3. I want suggestions to improve an Indian recipe that I know
Choose an option (1-3): 77
Invalid option. Please choose 1, 2, or 3.
1. I want the recipe for a specific Indian dish
2. I want a recipe for a specific Indian ingredient
3. I want suggestions to improve an Indian recipe that I know
Choose an option (1-3): 1
```

### Generating the Recipe

Based on the user's choice, we construct a prompt:

```python
# Make the prompt
if option == "1":
  dish = input("Enter the name of the Indian dish you want the recipe for: ")
  prompt = f"Suggest a detailed recipe for {dish}."
elif option == "2":
  ingredient = input("Enter the name of the Indian ingredient you want a recipe for: ")
  prompt = f"Suggest a detailed recipe using {ingredient}."
else:
  recipe = input("Enter the Indian recipe you want to improve:")
  prompt = f"Suggest improvements to the following Indian recipe: {recipe}."

# Example input
Enter the name of the Indian dish you want the recipe for: paneer pulao
```

Now we have all the necessary information to construct the phrase with which we will make the API request:

```python
# Add the user message
messages.append(
  {
    "role": "user",
    "content": prompt,
  }
)
```

Time to make the API call:

```python
# Make the API call
response = client.chat.completions.create(
  model=model,
  messages=messages,
)
```

Finally, we display the response:

```python
# Show the result
for choice in response.choices:
  print(choice.message.content)
```

### Example Output: Paneer Pulao Recipe

Here's an example of how the AI might generate a recipe for Paneer Pulao:

```markdown
Certainly! I'd be happy to provide you with a detailed recipe for Paneer Pulao, a delicious and popular Indian vegetarian dish. Here's how you can make it:

Ingredients:
- 2 cups Basmati rice
- 250g paneer, cubed
- 1 large onion, thinly sliced
- 1 cup mixed vegetables (carrots, peas, beans), chopped
- 2 tbsp ghee or oil
- 1 cinnamon stick
- 2-3 cloves
- 2-3 cardamom pods
- 1 bay leaf
- 1 tsp cumin seeds
- 2 green chilies, slit
- 1 tbsp ginger-garlic paste
- 1/4 cup cashews
- 2 tbsp raisins
- Salt to taste
- 4 cups water
- Fresh coriander leaves for garnish

Instructions:

1. Rinse the Basmati rice until the water runs clear. Soak it in water for 30 minutes, then drain.

2. Heat ghee or oil in a large pan or pressure cooker over medium heat.

3. Add the whole spices: cinnamon stick, cloves, cardamom pods, and bay leaf. Sauté for a minute until fragrant.

4. Add cumin seeds and let them sizzle for a few seconds.

5. Add sliced onions and green chilies. Sauté until the onions turn golden brown.

6. Add ginger-garlic paste and sauté for another minute.

7. Add the mixed vegetables and cook for 2-3 minutes.

8. Add the cubed paneer and sauté gently for a couple of minutes until it starts to lightly brown.

9. Add cashews and raisins, stirring for another minute.

10. Add the drained rice and gently stir to coat the rice with the spices and ghee.

11. Add salt to taste and 4 cups of water. Bring to a boil.

12. If using a pressure cooker, close the lid and cook for 2 whistles. If using a pan, cover and cook on low heat for about 15-20 minutes or until the rice is tender and water is absorbed.

13. Once cooked, let it rest for 5 minutes, then fluff the rice gently with a fork.

14. Garnish with fresh coriander leaves before serving.

Serve hot with raita or a side of your choice. Enjoy your homemade Paneer Pulao!

Tips for Improvement:
- For extra flavor, you can add a teaspoon of garam masala along with the other spices.
- If you prefer a richer taste, you can substitute half the water with vegetable broth.
- For a colorful variation, you can add a pinch of turmeric or a few strands of saffron to the rice while cooking.
- To make it more nutritious, you can add some spinach leaves or kale in the last few minutes of cooking.

Enjoy your delicious homemade Paneer Pulao!
```