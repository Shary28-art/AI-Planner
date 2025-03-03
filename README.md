# AI-Planner
import os
import streamlit as st
from langchain.prompts import PromptTemplate
from langchain.llms import GoogleGenerativeAI
from dotenv import load_dotenv

# Load API Key from .env
load_dotenv()
GOOGLE_API_KEY = os.getenv("GOOGLE_API_KEY")

# Set up Google GenAI model
llm = GoogleGenerativeAI(model="gemini-pro", google_api_key=GOOGLE_API_KEY)

# Streamlit UI
st.title("🗺️ AI-Powered Travel Planner")
st.write("Find the best travel options between any two cities!")

# Collect user input
source = st.text_input("Enter Source City", placeholder="e.g., Delhi")
destination = st.text_input("Enter Destination City", placeholder="e.g., Jaipur")

if st.button("Get Travel Options"):
    if not source or not destination:
        st.error("Please enter both source and destination.")
    else:
        # LangChain Prompt Template
        template = """
        You are an AI travel assistant. The user wants to travel from {source} to {destination}.
        Provide a list of available travel options including:
        - Travel mode (Cab, Train, Bus, Flight)
        - Estimated travel time
        - Approximate cost in INR
        Present it in a clear and structured format like a table.
        """

        prompt = PromptTemplate(
            input_variables=["source", "destination"],
            template=template
        )

        # Generate AI Response
        final_prompt = prompt.format(source=source, destination=destination)
        response = llm(final_prompt)

        # Display Result
        st.subheader(f"Travel Options from {source} to {destination}")
        st.write(response)
