# Notes on building a localized large model MVP based on LLM + RAG
# 0 Background
In an era where data privacy is paramount, building your ownLocal Language Model (LLM)A vital solution for companies and individuals alike.
# 1. Goal
According to the steps in this article, you can quickly build a local LLM +Retrieval-Augmented Generation (The RAG) system provides detailed steps and MVP codes to demonstrate to the outside world your LLM/AI Engineer capabilities and provide strong evidence for your transformation from a traditional software engineer to an AI/ML Engineer.

# 2 Technology Stack and Terminology
* Python 3: Python is a versatile programming language that you can use to write code for RAG applications.
* ChromaDB: A vector database for storing and managing our data embeddings.
To be: Download and serve the custom LLM on our local machine.
* Langchain: A development framework for building applications based on large language models (LLMs). It addresses the complexity of integrating, managing, and scaling LLMs within applications. In this local RAG system, it plays a central role in connecting large models, vector libraries, and application logic.

* LangChain = "Rapid Development of Chain-Based LLM Toolbox" for the Python Ecosystem
* Semantic Kernel = "Composable Intelligent Agent Platform" for the C#/.NET Ecosystem

# 3. Set up the environment
## 3.1 Installing Python Environment and Miniconda
Miniconda supports Python virtual environments in a smaller size and can support multiple virtual environments in one OS without conflict.
Its download address:https://www.anaconda.com/download/success





Then perform the following steps
Download moniconda
Install miniconda
Create a Python 3.10 virtual environment
second create -p D:\ProgramData\PythonVirtualEnvs\rag python=3.11

* -p or --prefix → Specify the full path of the environment
* python=3.10 → Use Python 3.10
<br>After the creation is complete, Conda will prompt you to confirm the installation of the package. Press y to confirm.
Activate the virtual environment
conda activate D:\ProgramData\PythonVirtualEnvs\rat


Install the required packages
pip install flask python-dotenv langchain langchain-community faiss-cpu chromadb unstructured pypdf pandas numpy scikit-learn matplotlib



## 3.2 Install Ollama and LLM
1 Download Ollama
Download link: https://ollama.com/download

2 Install Ollama and find the installation path
``` base
C:\INsers\<username>\AppData\Local\Programs\THEllama\llama.exe
```

3 Common commands
``` base
Cont. C:\INsers\<username>\AppData\Local\Programs\THEcalls\ # Switch path
ollama --Version # View ollama version
Ollama pull mistral # Install LLM mistral
ollama pull nomic-embed-text # Install embedding tool
ollama serve # Run the ollama model
```


![Ollama List](https://github.com/memoryfraction/Localized-LLM-RAG/blob/main/images/OLLAMA%20LIST.png?raw=true) 
<br>Ollama list renderings


# 4 Code part
app.py  This is the main file for the Flask application. It defines the routes for embedding files into the vector database and retrieving responses from the model.
embed.py- This module handles the embedding process, including saving uploaded files, loading and splitting data, and adding documents to the vector database.

query.py - This module processes user queries by generating multiple versions of the query, retrieving relevant documents, and providing answers based on the context.

Get_vector_db.py - This module initializes and returns a vector database instance for storing and retrieving document embeddings.

.env - Storing your environment variables

# 5 Results View
## 5.1 Startupapp.py
The following output appears, indicating success
``` base
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:8080
 * Running on http://192.168.1.145:8080
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 999-250-881
```

## 5.2 Sending Requests
![THINKPAD P14S hardware configuration](https://github.com/memoryfraction/Localized-LLM-RAG/blob/main/images/Thinkpad%20P14S%20Hardware.png?raw=true) 
Figure 1. THINKPAD P14S hardware configuration
5.2.1 Embed
Find a file to be uploaded, such as xxx.pdf, and use Postman's post request to upload the file.
ETF.pdf, file size: 17.915MB. Upload time: 1m 12.36s.
![Embed result](https://github.com/memoryfraction/Localized-LLM-RAG/blob/main/images/Embed%20Post%20Request.png?raw=true) 
Embed result image
5.2.2 Query
The query can be made to the LLM WebApi, and it is obvious that the answer comes from the uploaded "etf.pdf", because LLM is unlikely to know this information.
One request takes 3 minutes and 45 seconds. The machine configuration is as shown below.
![Query result](https://github.com/memoryfraction/Localized-LLM-RAG/blob/main/images/Query%20Post%20Request.png?raw=true) 
Figure Query return information
# QAs
1 How to view uploaded file fragments in chromaDb database?

2 How do I switch LLM models? For example, if I want to use a model that can process images, it is normal for the PDF to contain images.
Ollama pull your-image-model

In the .env file, set the LLM_MODEL variable to the name of the image recognition model.

Modify the code where the model is called to confirm that the new model supports the image input format. This may involve modifying the input preprocessing and calling logic.
```python
LLM_MODEL = os.getenv('LLM_MODEL', 'mistral')
llm = ChatOllama(model=LLM_MODEL)
```

And the embedded model in get_vector_db.py:
```python
TEXT_EMBEDDING_MODEL = os.getenv('TEXT_EMBEDDING_MODEL', 'nomic-embed-text')
embedding = OllamaEmbeddings(model=TEXT_EMBEDDING_MODEL)
```


Restart the service to apply the new model.
