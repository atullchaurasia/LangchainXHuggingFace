# LangChain X HuggingFace Integration

This project demonstrates how to integrate the Hugging Face models with LangChain to create powerful language model pipelines. The notebook uses the HuggingFaceEndpoint from the LangChain library to call custom endpoints deployed on Hugging Face.

## Setup and Requirements

### Libraries Required

To run this project, you'll need to install the following dependencies:

```bash
pip install langchain
pip install langchain_huggingface
pip install transformers
pip install huggingface_hub
pip install accelerate
pip install bitsandbytes
```

Make sure you also have a Hugging Face account to generate an API token.

### Getting Started

1. Clone the repository and navigate to the project directory:

```bash
git clone <repository-url>
cd <repository-name>
```

2. Install the required packages:

```bash
pip install -r requirements.txt
```

3. Set your Hugging Face API token by following the steps below:
   
   - Navigate to [Hugging Face](https://huggingface.co/), sign in, and get your token from the account settings.
   - In the notebook, the token will be retrieved using Google Colab's `userdata`:

    ```python
    from google.colab import userdata
    sec_key = userdata.get('HF_TOKEN')
    print(sec_key)
    ```

4. Initialize Hugging Face Endpoint using the `HuggingFaceEndpoint` class:

```python
from langchain_huggingface import HuggingFaceEndpoint

# Define your endpoint URL and Hugging Face token
endpoint_url = "https://your-custom-huggingface-endpoint-url"
api_key = "your-huggingface-api-key"

# Initialize the HuggingFaceEndpoint
llm = HuggingFaceEndpoint(
    endpoint_url=endpoint_url,
    headers={"Authorization": f"Bearer {api_key}"}
)
```

### Example Usage

1. Use the following code to create a chain and execute a prompt:

```python
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

prompt = PromptTemplate.from_template("What is {question}?")
llm_chain = LLMChain(prompt=prompt, llm=llm)

# Running the chain with a question
question = "LangChain"
response = llm_chain.run(question)
print(response)
```

### Note on Deprecated Methods

The LangChain `LLMChain` class and `run` method are deprecated in the latest version. Use `RunnableSequence` and `invoke` for newer versions of LangChain:

```python
from langchain.schema import RunnableSequence

# Creating the sequence
chain = RunnableSequence([prompt, llm])

# Use the invoke method
response = chain.invoke({"question": question})
print(response)
```

### Acknowledgements

This project leverages the following libraries:
- [LangChain](https://github.com/hwchase17/langchain)
- [Hugging Face Transformers](https://github.com/huggingface/transformers)
- [Hugging Face Hub](https://huggingface.co/)
