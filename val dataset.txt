import streamlit as st
from langchain.prompts import PromptTemplate
# Assuming LangChain version >= 0.1.0
from langchain_community.llms import HuggingFaceEndpoint  # Import the correct model class

# Your specific model setup
repo_id = 'mistralai/Mistral-7B-Instruct-v0.3'
sec_key = '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'  # Replace with your actual secret key

# Initialize the model with your specific parameters
llama2 = HuggingFaceEndpoint(repo_id=repo_id, max_length=256, temperature=0.01, token=sec_key)


# Define the function to generate a blog post
def generate_blog(blog_style, input_text, no_words):
    template = """
    write a blog for {blog_style} job profile for a topic {input_text} within {no_words} words.
    """
    prompt = PromptTemplate(template=template, input_variables=['blog_style', 'input_text', 'no_words'])

    # Format the prompt with user-provided values
    formatted_prompt = prompt.format(blog_style=blog_style, input_text=input_text, no_words=no_words)

    # Invoke the model with the formatted prompt text
    llm_chain = llama2(formatted_prompt)

    return llm_chain


# Streamlit app configuration
st.title('Blog Generator')

# Input fields with labels
blog_style = st.text_input('Blog Style', key="blog_style")
input_text = st.text_input('Topic', key="input_text")
no_words = st.number_input('Number of Words (100-1000)', min_value=100, max_value=1000, step=50, key="no_words")

# Generate blog button and conditional display
if st.button('Generate Blog'):
    if blog_style and input_text and no_words:
        blog_post = generate_blog(blog_style, input_text, no_words)
        st.write('Generated Blog Post:')
        st.write(blog_post)
    else:
        st.write('Please fill in all fields')
