# Customer Feedback Theme Classifier (Structured Output Version)

This program is an improved version of my earlier customer feedback classifier.  
It explores how to analyse customer feedback data **using structured outputs from modern LLMs**. I am mainly testing with GPT 4.1 and beyond models.

Instead of returning plain text, the model is asked to return **strict JSON**, which is then validated using **Pydantic models**.  

---

## Dataset Used

This project uses a small set of **hand-crafted sample rows** that mirror real customer support scenarios.  
These samples were inspired by the Kaggle dataset:

**BA Airlines Reviews Dataset**  
https://www.kaggle.com/datasets/chaudharyanshul/airline-reviews

Uploaded by *Chaudhary Anshul & Muskan Raisinghani*.




## stuff to note

-- Make sure you install the packages from requirements.txt
-- If you want to use this you need to supply your own API key and like I said, I am sure this works with many different models, but I have optimised this for GPT 4.1 and beyond.


## What It Does

The classification is structured into a few key parts:

- **Main Category** – Chosen from a predefined list (basically an Enum).  
  - **Justification** – A short explanation of why the model picked that category.

- **Sub Category** – More free-form and flexible, but still expected to relate to the main category.  
  - **Justification** – Why the model thinks this sub-category applies.  
  - **Linked Main Category ID** – An integer that maps directly back to the chosen main category.

- **Confidence Score** – How confident the LLM is in its result.  
  (This is honestly hit-and-miss, but still useful to include.)

These are represented as two separate Pydantic classes that feed into the main **TicketClassification** Pydantic model.

There are also a few built-in validation rules to keep things clean and consistent:

- Sub-category labels must be in **UPPER_SNAKE_CASE**  
- The **linked_main_category_id** must be valid and correctly mapped  
- If the model returns a **NOT_SURE** category, then no other categories are allowed on that row  
- Sub-categories that don’t tie to any main category are allowed (useful for discovering new themes),  
  but when that happens, the ID must come out as **9999**


  I have put the prompt in a seperate file called **prompt_data.py**. Please see that file if you want to view how its structured.
  -- In the event that a classification fails (so we dont even choose a 'NOT_SURE' category after 3 attemps) I have done a try/catch for this.

  ## What the output is

  A list of dictionaries. Each dictionary is like a JSON object.
  We can easily turn this into a dataframe if we want


## stuff to note

  -- Make sure you install the packages from requirements.txt
  -- If you want to use this you need to supply your own API key and like I said, I am sure this works with many different models, but I have optimised this for GPT 4.1 and beyond.


