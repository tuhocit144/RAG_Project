# Large Language Models (LLMs) và Retrieval Augmented Generation (RAG)

Large Language Models (LLMs) (Tạm dịch: Các mô hình ngôn ngữ lớn) là loại mô hình cho phép người dùng nhập vào một văn bản với nội dung bất kỳ, thường sẽ là các yêu cầu hoặc câu hỏi. Từ đó, mô hình sẽ trả về câu trả lời dưới dạng văn bản thỏa mãn yêu cầu của người dùng. Các ứng dụng phổ biến nhất về LLMs có thể kể đến như ChatGPT, Gemini...

## Retrieval Augmented Generation (RAG)

Retrieval Augmented Generation (RAG) là một kỹ thuật giúp LLMs cải thiện chất lượng kết quả tạo sinh bằng cách tích hợp nội dung truy vấn được từ một nguồn tài liệu nào đó để trả lời cho một câu hỏi đầu vào. Trong project này, chúng ta sẽ tìm hiểu cách sử dụng thư viện Chainlit để xây dựng một chương trình RAG cơ bản.

### Input:
- File tài liệu cần hỏi đáp
- Một câu hỏi liên quan đến nội dung tài liệu

### Output:
- Câu trả lời

## Các bước thực hiện

### 1. Cài đặt các gói thư viện cần thiết:

```bash
pip install -q transformers==4.41.2
pip install -q bitsandbytes==0.43.1
pip install -q accelerate==0.31.0
pip install -q langchain==0.2.5
pip install -q langchainhub==0.1.20
pip install -q langchain-chroma==0.1.1
pip install -q langchain-community==0.2.5
pip install -q langchain_huggingface==0.0.3
pip install -q python-dotenv==1.0.1
pip install -q pypdf==4.2.0
pip install -q numpy==1.24.4
```
### 2.	Tạo file app.py với các bước:
 #### + Xây dựng vector database
 #### + Import các thư viện cần thiết:
```bash
%%writefile app.py

import chainlit as cl
import torch

from chainlit.types import AskFileResponse

from transformers import BitsAndBytesConfig
from transformers import AutoTokenizer, AutoModelForCausalLM, pipeline
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_huggingface.llms import HuggingFacePipeline

from langchain_community.chat_message_histories import ChatMessageHistory
from langchain_community.document_loaders import PyPDFLoader, TextLoader
from langchain.chains import ConversationalRetrievalChain
from langchain.memory import ConversationBufferMemory
from langchain_chroma import Chroma
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser
from langchain import hub

```
 #### + Đọc file pdf:
   * Khởi tạo bộ tách văn bản (text splitter)
   * Khởi tạo instance vectorization
   * Khởi tạo vector database
   * Khởi tạo mô hình ngôn ngữ lớn
	
### 3.	Deploy chatbot bằng ngrok
Ngrok giống như localtunnel, cũng là công cụ tạo đường hầm (tunnel) giữa localhost và internet.
#### + Các bước thực hiện:
  * Vào https://ngrok.com/, tạo tài khoản và đăng nhập.
  *	Vào mục Your Authtoken, copy authtoken.
  * Tại cửa sổ Colab tạo cell mới và rõ lệnh:

  ```bash
  !pip install pyngrok -q
  ```
 ```bash
from pyngrok import ngrok
 ```
 ```bash
!ngrok config add-authtoken <mã-authtoken>
 ```
 ```bash
public_url = ngrok.connect(8000).public_url
print(public_url)
 ```
 ```bash
!chainlit run app.py
 ```
Đợi sau khi app.py chạy xong (output của các bạn xuất hiện dòng chữ xanh dương Your app is available at http://localhost:8000), vào đường dẫn của public_url để xem giao diện của ứng dụng.

