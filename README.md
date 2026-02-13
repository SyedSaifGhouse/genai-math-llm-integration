## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

## PROBLEM STATEMENT:
Design and implement a system where a user can input dimensions of a cylinder (radius and height),  
and the system calculates its volume by invoking a Python function using the function-calling capabilities of an LLM.

## DESIGN STEPS:

1. Import the required libraries (`groq`, `json`, `math`) and initialize the LLM client.
2. Create a function (`calculate_cylinder_volume`) to compute the volume of a cylinder and define its schema for function calling.
3. Send the user query to the LLM, process the response, and execute the function if triggered, then return the final result to the user.

### PROGRAM:

```
Name : SYED SAIF SYED GHOUSE
Reg no: 212224230286
```

```py
from groq import Groq
import json
import math

# Initialize client
client = Groq(api_key="api_key")


# Volume function
def calculate_cylinder_volume(radius: float, height: float) -> float:
    return math.pi * radius * radius * height


# Function Schema for Function Calling
function_schema = {
    "name": "calculate_cylinder_volume",
    "description": "Calculate the volume of a cylinder when radius and height are provided.",
    "parameters": {
        "type": "object",
        "properties": {
            "radius": {"type": "number", "description": "Radius of cylinder"},
            "height": {"type": "number", "description": "Height of cylinder"},
        },
        "required": ["radius", "height"],
    },
}

print("Cylinder Volume Chat System\n")

user_query = input("User: ")

# Send request to LLM
response = client.chat.completions.create(
    model="llama-3.1-8b-instant",
    messages=[{"role": "user", "content": user_query}],
    functions=[function_schema],
    function_call="auto"
)

message = response.choices[0].message

if message.function_call:
    args = json.loads(message.function_call.arguments)
    volume = calculate_cylinder_volume(args["radius"], args["height"])

    print(f"\nLLM Function Output: {{'volume': {message}}}")
    print(f"Bot: The volume of the cylinder is {volume:.2f}.")

else:
    print("\nBot:", message.content)

```
### OUTPUT:

<img width="1734" height="150" alt="image" src="https://github.com/user-attachments/assets/f2c1e40a-cbdf-48fb-9e8a-5ee60a63cc47" />

### RESULT:

A Python program that calculates the volume of a cylinder has been successfully designed, implemented, and integrated with a chat completion system using the LLM's function-calling feature.
