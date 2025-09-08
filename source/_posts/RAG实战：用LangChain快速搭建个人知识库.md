---
title: RAG实战：用LangChain快速搭建个人知识库
date: 2025-08-16 16:35:41
tags:
  - langchain
  - rag
typora-root-url: ./RAG实战：用LangChain快速搭建个人知识库
---

# RAG实战：用LangChain快速搭建个人知识库

# 前言

本人在物联网（IoT）相关领域工作一年多，最近一直在看大模型和MQTT相关的知识。相比于枯燥的理论学习，我更信奉**“Learning by Doing”**，所以我想利用时下大火RAG技术搭建一个MQTT协议的个人知识库，下面将详细记录从环境搭建到应用上线的每一步，展示一个完整RAG项目的实现细节。

# 背景知识

## RAG 简介

RAG（Retrieval-Augmented Generation，检索增强生成） 是一种结合信息检索与生成模型的技术。其核心思想是：在生成答案前，先从外部知识库（如文档、数据库、互联网）中检索相关证据，再基于检索结果和用户输入生成更准确、可靠的回答。如下图所示为一个最简RAG示意图。

![图片](640.png)

从形态上说，LLM充当大脑角色用于生成答案，检索充当触手角色用于收集证据。RAG就是一个带触手（外挂知识库）的大模型系统。

# 环境准备

## 先决条件

- **Python 3.9+**: 确保系统已安装 Python。
- **Docker**: 这是运行 PostgreSQL + pgvector 最简单、最推荐的方式。
- **OpenAI API Key**: 前往 [OpenAI 官网](https://platform.openai.com/api-keys) 创建一个 API Key。（推荐白嫖地址：https://github.com/chatanywhere/GPT_API_free）

## 核心依赖项

```
# --- Core Framework ---
langchain              # RAG 流程的核心编排框架

# --- LangChain Integrations ---
langchain-postgres     # 用于与 PGVector 数据库交互的最新、推荐库
langchain-openai       # 用于连接 OpenAI 的 LLM 和 Embedding 模型

# --- Database Driver ---
psycopg                # PostgreSQL 的 Python 数据库驱动程序，供 langchain-postgres 使用

# --- User Interface ---
gradio                 # 快速构建交互式 Web UI

# --- Data Handling ---
pypdf                  # 用于加载和解析 PDF 文档
tiktoken               # OpenAI 官方的分词器，用于计算 token 数量

# --- Configuration ---
python-dotenv          # 用于从 .env 文件加载环境变量
```

将以下内容保存到项目的 `requirements.txt` 文件中，以便于一键安装 (`pip install -r requirements.txt`)。

## 项目结构：

```sh
mqtt-rag-project/
├── .env                              # 环境变量文件
├── app.py                            # Web应用与问答服务
├── hivemq-ebook-mqtt-essentials.pdf  # MQTT PDF 文档
├── ingest.py                         # 数据入库脚本
└── start-pgvector.sh                 # pgvector的启动脚本
```

## pgvector向量数据库 docker安装

先安装postgresql客户端

```sh
yum install postgresql
```

### start-pgvector.sh :

```sh
docker run --name pgvector-db \
  -e POSTGRES_USER=root \
  -e POSTGRES_PASSWORD=root \
  -e POSTGRES_DB=pgvector \
  -p 5432:5432 \
  -d pgvector/pgvector:pg17
```

验证有没有成功安装

```sh
psql -h localhost -p 5432 -U root pgvector
```

输入密码后成功进去就OK。

## pgadmin 安装（可选）

官网：https://www.pgadmin.org/download/

使用pgadmin可视化的看到数据库中的向量存储展示。

# 项目细节：

## 整体流程：

这是整个项目的完整工作流程图，清晰地展示了数据入库和用户问答两个阶段。

```plantuml

@startuml

title MQTT RAG 知识库 - 全流程图

actor Developer
actor User

box "离线数据处理 (Offline Ingestion)" #LightBlue
    participant "ingest.py" as Ingest
    participant "Embedding Model\n(OpenAI)" as Embed
    database "PGVector Database" as DB
end box

box "在线问答服务 (Online Querying)" #LightGreen
    participant "Gradio UI" as UI
    participant "LangChain RAG Chain\n(app.py)" as RAG
    participant "LLM\n(GPT-4o)" as LLM
end box


== 📖 1. 数据入库阶段 (由 Developer 执行) ==

Developer -> Ingest: 运行 python ingest.py
activate Ingest

Ingest -> Ingest: 1. 加载并分割\n"mqtt.pdf"
Ingest -> Embed: 2. 对每个文本块\n请求向量化
activate Embed
Embed --> Ingest: 返回文本向量
deactivate Embed

Ingest -> DB: 3. (清空旧数据后)\n存储文本块和向量
activate DB
DB --> Ingest: 存储成功
deactivate DB

Ingest --> Developer: 打印 "入库完成" 日志
deactivate Ingest


== 💬 2. 用户问答阶段 (由 User 使用) ==

User -> UI: 1. 在聊天框输入问题
activate UI
UI -> RAG: 2. 发送问题
deactivate UI
activate RAG

RAG -> Embed: 3. 将用户问题向量化
activate Embed
Embed --> RAG: 返回问题向量
deactivate Embed

RAG -> DB: 4. 使用问题向量\n进行相似度搜索
activate DB
DB --> RAG: 5. 返回最相关的\nK个文本块 (上下文)
deactivate DB

RAG -> RAG: 6. 构建 Prompt:\n(上下文 + 原始问题)

RAG -> LLM: 7. 发送增强后的 Prompt
activate LLM
LLM --> RAG: 8. **流式**返回生成的答案
deactivate LLM

RAG -> UI: 9. 将答案**流式**传输到前端
activate UI
UI -> User: 10. 在聊天框中实时\n显示答案 (打字机效果)
deactivate UI
deactivate RAG

@enduml

```

## .env （环境变量）

```
# .env 文件
OPENAI_API_KEY="xxxx"
OPENAI_BASE_URL="xxxxx"
DATABASE_URL="postgresql://root:root@localhost:5432/pgvector"
```

## hivemq-ebook-mqtt-essentials.pdf(MQTT相关的书籍)

hivemq组件推出的一本mqtt相关的书籍，需要把这本书解析为知识库。

## ingest.py (数据入库脚本)

```python
import os
import logging
import psycopg
from dotenv import load_dotenv

from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores.pgvector import PGVector

# --- 1. ⚙️ 配置日志系统 ---
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

# --- 2. 🔑 加载环境变量和配置 ---
logging.info("🚀 开始数据入库流程...")
load_dotenv()
logging.info("✅ .env 文件加载成功")

# 从环境变量中安全地获取配置
PDF_PATH = "hivemq-ebook-mqtt-essentials.pdf"  # 你可以把它也放入 .env
COLLECTION_NAME = "mqtt_docs"
CONNECTION_STRING = os.getenv("DATABASE_URL")
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
OPENAI_BASE_URL = os.getenv("OPENAI_BASE_URL")

# 检查关键配置是否存在
if not all([CONNECTION_STRING, OPENAI_API_KEY, OPENAI_BASE_URL]):
    error_msg = "❌ 关键环境变量缺失！请检查 .env 文件是否包含 DATABASE_URL, OPENAI_API_KEY, OPENAI_BASE_URL"
    logging.error(error_msg)
    raise ValueError(error_msg)
logging.info("👍 所有环境变量配置加载完毕")


def ingest_data():
    """
    加载、分割、嵌入并存储PDF文档数据到PGVector。
    """
    # 检查PDF文件是否存在
    if not os.path.exists(PDF_PATH):
        logging.error(f"🔥 错误: PDF文件未找到于 {PDF_PATH}")
        return

    connection = None
    try:
        # --- 3. 📄 加载PDF文档 ---
        logging.info(f"📄 正在加载文档: {PDF_PATH}...")
        loader = PyPDFLoader(PDF_PATH)
        documents = loader.load()
        logging.info(f"✅ 成功加载 {len(documents)} 页文档。")

        # --- 4. 🔪 分割文档为文本块 ---
        logging.info("🔪 正在分割文档为文本块...")
        text_splitter = RecursiveCharacterTextSplitter(
            chunk_size=1000,
            chunk_overlap=150,
            length_function=len
        )
        chunks = text_splitter.split_documents(documents)
        logging.info(f"✅ 文档被分割成 {len(chunks)} 个文本块。")

        # --- 5. 🧠 创建文本嵌入模型 ---
        logging.info("🧠 正在初始化 OpenAI Embedding 模型 (text-embedding-3-small)...")
        embeddings = OpenAIEmbeddings(
            model="text-embedding-3-small",
            api_key=OPENAI_API_KEY,
            base_url=OPENAI_BASE_URL
        )
        logging.info("✅ 嵌入模型初始化完成。")

        # --- 6. 📦 将数据存入 PGVector ---
        logging.info(f"🔗 正在连接到数据库: {CONNECTION_STRING.split('@')[-1]}")
        connection = psycopg.connect(CONNECTION_STRING)
        logging.info("🎉 数据库连接成功！")

        logging.info(f"📦 准备将 {len(chunks)} 个文本块存入集合 '{COLLECTION_NAME}'...")

        # 使用 from_documents 方法，并设置 pre_delete_collection=True
        # 这会先删除同名旧集合（如果存在），确保数据从零开始，非常适合重新入库
        PGVector.from_documents(
            documents=chunks,
            embedding=embeddings,
            collection_name=COLLECTION_NAME,
            connection_string=CONNECTION_STRING,
            pre_delete_collection=True,  # ✨ 最佳实践: 保证每次都是全新入库
        )

        logging.info("🎉 数据处理流程圆满完成！")
        logging.info(f"✅ {len(chunks)} 个文档块已成功存入集合 '{COLLECTION_NAME}'。")

    except Exception as e:
        logging.error(f"🔥 数据处理流程中发生严重错误: {e}", exc_info=True)
    finally:
        # 确保数据库连接在使用后被关闭
        if connection:
            connection.close()
            logging.info("🚪 数据库连接已关闭。")


if __name__ == "__main__":
    ingest_data()

```

流程总结：

```mermaid
%% `ingest.py`
graph TD
    subgraph "准备阶段 (Preparation)"
        A[开始 Ingestion 脚本] --> B["配置与加载<br>(Logging, .env)"];
    end

    subgraph "数据处理 (Processing)"
        B --> C{PDF 文件存在?};
        C -- 否 --> D[记录错误并退出];
        C -- 是 --> E["加载 PDF 文档<br>(使用 PyPDFLoader)"];
        E --> F["分割文档为文本块 (Chunks)<br>(使用 RecursiveCharacterTextSplitter)"];
        F --> G["初始化嵌入模型<br>(OpenAI text-embedding-3-small)"];
    end
    
    subgraph "数据库操作 (Database Operations)"
        G --> H["连接到 PostgreSQL 数据库<br>(使用 psycopg)"];
        H --> I[调用 PGVector.from_documents];
        
        subgraph I [内部流程]
            direction TB
            I1{pre_delete_collection=True?};
            I1 -- 是 --> I2[清空/删除旧的 Collection];
            I1 -- 否 --> I3;
            I2 --> I3["对于每一个 Chunk..."];
            I3 --> I4["调用 OpenAI API<br>将文本转换为 Vector"];
            I4 --> I5["将 (文本 + Vector)<br>插入数据库"];
            I5 --> I3;
        end
    end

    subgraph "收尾阶段 (Finalization)"
         I --> J["记录成功日志"];
         J --> K["关闭数据库连接<br>(在 finally 块中确保执行)"];
         K --> L[流程结束];
    end
```



这个脚本是一个**一次性**的、**离线**运行的程序，作用就构建知识库。

- **🎯 目的**: 将非结构化的 PDF 文档转换为结构化的、可供快速检索的向量数据。
- 核心流程
  1. **加载 (Load)**: 读取指定的 MQTT PDF 文档内容。
  2. **分割 (Split)**: 将整个文档的长文本切割成更小的、带有上下文重叠的文本块 (Chunks)。
  3. **嵌入 (Embed)**: 调用 OpenAI 的 `text-embedding-3-small` 模型，将每一个文本块转换成一个高维数学向量。
  4. **存储 (Store)**: 将文本块和其对应的向量一起存入 PostgreSQL 数据库的 `pgvector` 扩展中，并使用 `pre_delete_collection=True` 确保每次都是全新的数据。

运行：

```sh
python ingest.py 
2025-08-16 17:17:22 - INFO - 🚀 开始数据入库流程...
2025-08-16 17:17:22 - INFO - ✅ .env 文件加载成功
2025-08-16 17:17:22 - INFO - 👍 所有环境变量配置加载完毕
2025-08-16 17:17:22 - INFO - 📄 正在加载文档: hivemq-ebook-mqtt-essentials.pdf...
2025-08-16 17:17:24 - INFO - ✅ 成功加载 90 页文档。
2025-08-16 17:17:24 - INFO - 🔪 正在分割文档为文本块...
2025-08-16 17:17:24 - INFO - ✅ 文档被分割成 317 个文本块。
2025-08-16 17:17:24 - INFO - 🧠 正在初始化 OpenAI Embedding 模型 (text-embedding-3-small)...
2025-08-16 17:17:24 - INFO - ✅ 嵌入模型初始化完成。
2025-08-16 17:17:24 - INFO - 🔗 正在连接到数据库: localhost:5432/pgvector
2025-08-16 17:17:24 - INFO - 🎉 数据库连接成功！
2025-08-16 17:17:24 - INFO - 📦 准备将 317 个文本块存入集合 'mqtt_docs'...
2025-08-16 17:17:27 - INFO - HTTP Request: POST https://api.chatanywhere.tech/embeddings "HTTP/1.1 200 OK"
/usr/local/lib/python3.11/site-packages/langchain_community/vectorstores/pgvector.py:490: LangChainPendingDeprecationWarning: Please use JSONB instead of JSON for metadata. This change will allow for more efficient querying that involves filtering based on metadata. Please note that filtering operators have been changed when using JSONB metadata to be prefixed with a $ sign to avoid name collisions with columns. If you're using an existing database, you will need to create a db migration for your metadata column to be JSONB and update your queries to use the new operators. 
  store = cls(
2025-08-16 17:17:35 - INFO - 🎉 数据处理流程圆满完成！
2025-08-16 17:17:35 - INFO - ✅ 317 个文档块已成功存入集合 'mqtt_docs'。
2025-08-16 17:17:35 - INFO - 🚪 数据库连接已关闭。
```

pgvector 有告警是因为 LangChain 推荐使用性能更好的 `JSONB` 格式来存储元数据。

### 数据库向量：

用pgadmin工具可以看到数据库里面创建了2张表：

![image-20250816182559256](image-20250816182559256.png)

**`langchain_pg_collection` (集合目录表)**

- **作用**: 记录了你创建的所有向量集合（Collections）的名称和唯一ID。
- **内容**: 每行代表一个独立的知识库（例如 `mqtt_docs`）。

**`langchain_pg_embedding` (嵌入数据表)**

- **作用**: 存储了**所有集合**的全部数据。
- **内容**: 每行包含一个具体的文本块（document）、其对应的向量（embedding）以及一个 `collection_id`，用于指明这条数据属于哪个集合。

```sql
CREATE TABLE IF NOT EXISTS public.langchain_pg_collection
(
    -- 字段注释 --
    name      character varying,  -- 集合的易读名称，由开发者在代码中指定 (例如 'mqtt_docs')。
    cmetadata json,               -- (Collection Metadata) 用于存储关于整个集合的元数据的 JSON 字段。例如，可以存储集合的描述、来源信息等。
    uuid      uuid NOT NULL,      -- 集合的唯一标识符 (UUID)，作为这张表的主键 (Primary Key)。它被 `langchain_pg_embedding` 表用作外键来关联数据。

    -- 约束注释 --
    CONSTRAINT langchain_pg_collection_pkey PRIMARY KEY (uuid) -- 将 `uuid` 字段设置为主键，确保每个集合都有一个唯一的、非空的标识符。
);

CREATE TABLE IF NOT EXISTS public.langchain_pg_embedding
(
    -- 字段注释 --
    collection_id uuid,               -- 外键 (Foreign Key)，关联到 `langchain_pg_collection` 表中的 `uuid`。它标识了这条数据属于哪一个集合。
    embedding     vector,             -- 文本块的向量表示。这是由 `pgvector` 扩展提供的数据类型，用于进行高效的相似度搜索。
    document      character varying,  -- 原始的文本块内容。这是 RAG 流程中检索到的、将提供给 LLM 作为上下文的实际文本。
    cmetadata     json,               -- (Content Metadata) 关于单个文本块的元数据的 JSON 字段。通常用于存储来源信息 (如文件名、页码) 以便追溯和过滤。较新版本中，此字段类型已升级为 `JSONB` 以提高查询性能。
    custom_id     character varying,  -- 一个可选的、由用户自定义的 ID 字段。如果开发者在添加文档时提供了 `ids` 参数，这些 ID 会被存储在这里，方便与外部系统进行关联或进行特定 ID 的数据操作。
    uuid          uuid NOT NULL,      -- 这条嵌入数据记录自身的唯一标识符 (UUID)，作为这张表的主键。

    -- 约束注释 --
    CONSTRAINT langchain_pg_embedding_pkey PRIMARY KEY (uuid), -- 将 `uuid` 字段设置为主键，确保表中的每一条数据记录都是唯一的。

    CONSTRAINT langchain_pg_embedding_collection_id_fkey FOREIGN KEY (collection_id) -- 定义了 `collection_id` 是一个外键，它必须引用 `langchain_pg_collection` 表中真实存在的 `uuid`。
        REFERENCES public.langchain_pg_collection (uuid) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE CASCADE -- 这是一个关键的级联删除规则：当一个集合从 `langchain_pg_collection` 表中被删除时，所有属于该集合的数据将自动从这张表中被删除，保证了数据的完整性。
);
```



用sql看下数据:

```sh
pgvector=# select * from langchain_pg_collection;
   name    | cmetadata |                 uuid                 
-----------+-----------+--------------------------------------
 mqtt_docs | null      | 6b065562-e20d-4758-8097-8459dd3618db
(1 row)
pgvector=#  select count(*) from langchain_pg_embedding;
 count 
-------
   317
(1 row)
```

langchain_pg_embedding 数量和日志里面对的上的，langchain_pg_embedding 的表数据太长了，下图展示一下：

![image-20250816185704062](image-20250816185704062.png)

这里直观的看到文本转为为向量并存储到数据库中了。

## `app.py` (Web应用与问答服务)

```python
import os
import logging
import gradio as gr
import psycopg
from dotenv import load_dotenv

from langchain_community.vectorstores.pgvector import PGVector
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain.prompts import PromptTemplate
from langchain.schema.runnable import RunnablePassthrough, RunnableLambda
from langchain.schema.output_parser import StrOutputParser

# --- 1. ⚙️ 配置日志系统 ---
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

# --- 2. 🔑 加载环境变量和配置 ---
logging.info("🚀 应用开始启动...")
load_dotenv()
logging.info("✅ .env 文件加载成功")

CONNECTION_STRING = os.getenv("DATABASE_URL")
COLLECTION_NAME = "mqtt_docs"
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
OPENAI_BASE_URL = os.getenv("OPENAI_BASE_URL")

# 检查关键配置是否存在
if not all([CONNECTION_STRING, COLLECTION_NAME, OPENAI_API_KEY, OPENAI_BASE_URL]):
    error_msg = "❌ 关键环境变量缺失！请检查 .env 文件是否包含 DATABASE_URL, OPENAI_API_KEY, OPENAI_BASE_URL"
    logging.error(error_msg)
    raise ValueError(error_msg)
logging.info("👍 所有环境变量配置加载完毕")

# --- 3. 🔗 初始化数据库连接 ---
try:
    logging.info(f"🔗 正在连接到数据库: {CONNECTION_STRING.split('@')[-1]}")
    connection = psycopg.connect(CONNECTION_STRING)
    logging.info("🎉 数据库连接成功！")
except Exception as e:
    logging.error(f"🔥 数据库连接失败: {e}")
    raise

# --- 4. 🧠 初始化 AI 模型 ---
logging.info("🧠 正在初始化 OpenAI 模型...")
llm = ChatOpenAI(
    base_url=OPENAI_BASE_URL,
    api_key=OPENAI_API_KEY,
    model="gpt-4o",
    temperature=0.1,
    streaming=True
)
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    api_key=OPENAI_API_KEY,
    base_url=OPENAI_BASE_URL
)
logging.info("🤖 LLM 和 Embedding 模型初始化完成")

# --- 5. 📚 初始化向量存储和检索器 ---
logging.info(f"📚 正在连接到向量集合: '{COLLECTION_NAME}'")
vectorstore = PGVector(
    connection_string=CONNECTION_STRING,
    collection_name=COLLECTION_NAME,
    embedding_function=embeddings,
)

# 返回10个最相关的文档
retriever = vectorstore.as_retriever(search_kwargs={'k': 10})
logging.info("✅ 向量存储和检索器准备就绪")

# --- 6. 🛠️ 构建 RAG 链 (带日志记录) ---

# 提示模板
template = """
你是一个关于MQTT协议的技术专家。请根据下面提供的上下文信息，用中文、清晰、准确地回答用户的问题。
如果上下文中没有足够的信息来回答问题，请明确说明“根据我所掌握的知识，无法回答这个问题”，不要编造答案。

上下文:
{context}

问题:
{question}

回答:
"""
prompt = PromptTemplate.from_template(template)


def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)


def log_retrieved_documents(docs):
    """一个用于在链中记录检索到的文档的函数"""
    logging.info(f"🔍 向量检索完成，找到 {len(docs)} 个相关文档:")
    for i, doc in enumerate(docs):
        content_preview = doc.page_content.replace('\n', ' ').strip()[:120]
        page_num = doc.metadata.get('page', 'N/A')
        logging.info(f"  📄 文档 {i + 1} (页码: {page_num}): '{content_preview}...'")
    return docs


logging.info("⛓️ 正在构建 RAG 链...")
rag_chain = (
        {
            "context": retriever | RunnableLambda(log_retrieved_documents) | format_docs,
            "question": RunnablePassthrough()
        }
        | prompt
        | llm
        | StrOutputParser()
)
logging.info("✅ RAG 链构建完成")


# --- 7. 💬 定义 Gradio 聊天功能 ---
def chat_function(message, history):
    logging.info(f"💬 收到用户新问题: '{message}'")

    # 流式处理
    response_generator = rag_chain.stream(message)
    partial_message = ""
    logging.info("✍️ 开始向用户流式传输回答...")
    for chunk in response_generator:
        partial_message += chunk
        yield partial_message
    logging.info("✅ 流式传输完成")


# --- 8. 🌐 启动 Gradio 应用 ---
if __name__ == "__main__":
    logging.info("🌐 准备启动 Gradio Web UI...")
    demo = gr.ChatInterface(
        fn=chat_function,
        title="MQTT 协议知识库 🤖",
        description="""
        这是一个基于 MQTT 协议文档的 RAG 问答机器人。
        你可以用中文提问关于 MQTT 协议的任何问题，例如：
        - "MQTT的QoS等级有哪些？分别解释一下。"
        - "CONNECT报文的结构是怎样的？"
        - "什么是遗嘱消息（Will Message）？"
        """,
        type="messages",
        chatbot=gr.Chatbot(height=600),
        textbox=gr.Textbox(placeholder="请输入你关于MQTT协议的问题...", container=False, scale=7),
        theme="soft",
    )
    demo.launch()
```

流程总结：



```mermaid
%% `app.py`
graph TD
    subgraph "阶段一: 应用启动与初始化"
        A[启动 app.py]
        A --> B["配置与加载<br>(Logging, .env)"]
        B --> C["建立持久的<br>数据库连接 (psycopg)"]
        C --> D["初始化 AI 模型<br>(LLM: GPT-4o & Embeddings)"]
        D --> E["初始化向量存储<br>并创建 Retriever"]
        E --> F["构建 RAG 链<br>(使用 LangChain LCEL)"]
        F --> G["启动 Gradio Web UI<br>(应用进入监听状态)"]
    end

    %% 添加一个从阶段一到阶段二的逻辑连接线
    G -. "用户开始交互" .-> User

    subgraph "阶段二: 实时问答流程"
        User["User"]
        UI[Gradio UI]
        RAG["LangChain RAG Chain<br>(在 app.py 中运行)"]
        DB["PGVector Database"]
        LLM["LLM (GPT-4o)"]

        User -- 1.输入问题 --> UI
        UI -- 2.将问题发送给chat_function--> RAG
        RAG -- 3.向量化问题并检索--> DB
        DB -- "4.返回 K 个最相关的<br>文本块 (上下文)" --> RAG
        RAG -- "5.构建最终 Prompt:<br>[上下文] + [原始问题]" --> LLM
        LLM -- 6.<b>流式</b>生成答案 --> RAG
        RAG -- "7.将答案 <b>流式</b> 传输回 UI" --> UI
        UI -- "8.实时更新聊天窗口<br>(打字机效果)" --> User
    end
```



这个脚本是一个**持续运行**的、**在线**提供服务的程序，它构成了用户与之交互的后端和前端。

- **🎯 目的**: 接收用户问题，利用已构建的知识库生成精准回答，并通过 Web 界面展示。
- 核心流程
  1. **初始化 (Initialize)**: 在启动时，加载所有配置，连接数据库，并准备好大语言模型 (GPT-4o)、嵌入模型和向量检索器。
  2. **构建RAG链 (Build RAG Chain)**: 使用 LangChain Expression Language (LCEL) 定义一个清晰的、可流式处理的问答逻辑链。
  3. **提供Web界面 (Provide Web UI)**: 启动一个 Gradio 服务，创建一个用户友好的聊天窗口。
  4. 处理用户请求 (Handle User Request)
     - 接收用户输入的问题。
     - **检索 (Retrieve)**: 将问题向量化，并在 `pgvector` 中快速找到最相似的 N 个文本块（即最相关的上下文）。
     - **增强 (Augment)**: 将检索到的上下文和原始问题组合成一个详细的提示 (Prompt)。
     - **生成 (Generate)**: 将这个增强后的提示发送给 GPT-4o，让它基于提供的上下文生成回答。
     - **流式返回 (Stream)**: 将 GPT-4o 生成的答案以流的形式实时传输回前端，实现打字机效果。

运行：

```sh
# python app.py
2025-08-16 18:59:23 - INFO - 🚀 应用开始启动...
2025-08-16 18:59:23 - INFO - ✅ .env 文件加载成功
2025-08-16 18:59:23 - INFO - 👍 所有环境变量配置加载完毕
2025-08-16 18:59:23 - INFO - 🔗 正在连接到数据库: localhost:5432/pgvector
2025-08-16 18:59:23 - INFO - 🎉 数据库连接成功！
2025-08-16 18:59:23 - INFO - 🧠 正在初始化 OpenAI 模型...
2025-08-16 18:59:23 - INFO - 🤖 LLM 和 Embedding 模型初始化完成
2025-08-16 18:59:23 - INFO - 📚 正在连接到向量集合: 'mqtt_docs'
/root/project/mqtt-rag-project/app.py:64: LangChainPendingDeprecationWarning: This class is pending deprecation and may be removed in a future version. You can swap to using the `PGVector` implementation in `langchain_postgres`. Please read the guidelines in the doc-string of this class to follow prior to migrating as there are some differences between the implementations. See <https://github.com/langchain-ai/langchain-postgres> for details about the new implementation.
  vectorstore = PGVector(
/root/project/mqtt-rag-project/app.py:64: LangChainPendingDeprecationWarning: Please use JSONB instead of JSON for metadata. This change will allow for more efficient querying that involves filtering based on metadata. Please note that filtering operators have been changed when using JSONB metadata to be prefixed with a $ sign to avoid name collisions with columns. If you're using an existing database, you will need to create a db migration for your metadata column to be JSONB and update your queries to use the new operators. 
  vectorstore = PGVector(
2025-08-16 18:59:23 - INFO - ✅ 向量存储和检索器准备就绪
2025-08-16 18:59:23 - INFO - ⛓️ 正在构建 RAG 链...
2025-08-16 18:59:23 - INFO - ✅ RAG 链构建完成
2025-08-16 18:59:23 - INFO - 🌐 准备启动 Gradio Web UI...
/root/project/mqtt-rag-project/app.py:147: UserWarning: You have not specified a value for the `type` parameter. Defaulting to the 'tuples' format for chatbot messages, but this is deprecated and will be removed in a future version of Gradio. Please set type='messages' instead, which uses openai-style dictionaries with 'role' and 'content' keys.
  chatbot=gr.Chatbot(height=600),
/usr/local/lib/python3.11/site-packages/gradio/chat_interface.py:321: UserWarning: The type of the gr.Chatbot does not match the type of the gr.ChatInterface.The type of the gr.ChatInterface, 'messages', will be used.
  warnings.warn(
* Running on local URL:  http://127.0.0.1:7860
2025-08-16 18:59:24 - INFO - HTTP Request: GET http://127.0.0.1:7860/gradio_api/startup-events "HTTP/1.1 200 OK"
2025-08-16 18:59:24 - INFO - HTTP Request: HEAD http://127.0.0.1:7860/ "HTTP/1.1 200 OK"
* To create a public link, set `share=True` in `launch()`.
2025-08-16 18:59:26 - INFO - HTTP Request: GET https://api.gradio.app/pkg-version "HTTP/1.1 200 OK"
```

服务成功启动了，看下web页面：

![image-20250816190128801](image-20250816190128801.png)

现在让我们提问几个MQTT相关的知识点，看下有没有成功检索刚刚创建的知识库，日志里面会详细打印数据。

![image-20250816190506250](image-20250816190506250.png)

日志输出：

```sh
2025-08-16 19:04:15 - INFO - HTTP Request: HEAD http://127.0.0.1:7860/ "HTTP/1.1 200 OK"
2025-08-16 19:04:15 - INFO - HTTP Request: GET https://api.gradio.app/pkg-version "HTTP/1.1 200 OK"
2025-08-16 19:04:24 - INFO - 💬 收到用户新问题: 'mqtt 是什么？'
2025-08-16 19:04:24 - INFO - ✍️ 开始向用户流式传输回答...
2025-08-16 19:04:25 - INFO - HTTP Request: POST https://api.chatanywhere.tech/embeddings "HTTP/1.1 200 OK"
2025-08-16 19:04:25 - INFO - 🔍 向量检索完成，找到 10 个相关文档:
2025-08-16 19:04:25 - INFO -   📄 文档 1 (页码: 2): 'Chapter 1: Introduction to MQTT MQTT is a lightweight messaging protocol originally designed for communication in constr...'
2025-08-16 19:04:25 - INFO -   📄 文档 2 (页码: 4): 'While it formerly stood for MQ Telemetry  Transport, where MQ referred to the MQ  Series, a product IBM developed to sup...'
2025-08-16 19:04:25 - INFO -   📄 文档 3 (页码: 4): 'cases. Over the next ten years, IBM used the protocol internally until  they released MQTT 3.1 as a royalty-free version...'
2025-08-16 19:04:25 - INFO -   📄 文档 4 (页码: 4): 'Real-World Applications and Use Cases of  MQTT: An Overview MQTT is used extensively in IoT, Industrial IoT (IIoT), and ...'
2025-08-16 19:04:25 - INFO -   📄 文档 5 (页码: 77): 'managed cloud MQTT broker. Explore them now! MQTT HTTP Full name MQTT (the OASIS standardization  group decided it would...'
2025-08-16 19:04:25 - INFO -   📄 文档 6 (页码: 3): 'Another important aspect of the protocol is that MQTT is  extremely easy to implement on the client side. Ease of use  w...'
2025-08-16 19:04:25 - INFO -   📄 文档 7 (页码: 8): 'established, clients can publish messages to topics  or subscribe to topics to receive messages from other  clients. • K...'
2025-08-16 19:04:25 - INFO -   📄 文档 8 (页码: 20): 'IoT devices. The foundation of this communication is  the MQTT connection, which enables devices to securely  and reliab...'
2025-08-16 19:04:25 - INFO -   📄 文档 9 (页码: 69): 'and brokers. Its Keep Alive mechanism minimizes energy and  bandwidth usage, enabling users to establish cost-effective,...'
2025-08-16 19:04:25 - INFO -   📄 文档 10 (页码: 3): 'MQTT uses a binary message format for communication between clients and servers (brokers). This is in contrast to other ...'
2025-08-16 19:04:27 - INFO - HTTP Request: POST https://api.chatanywhere.tech/chat/completions "HTTP/1.1 200 OK"
2025-08-16 19:04:27 - INFO - ✅ 流式传输完成
2025-08-16 19:04:48 - INFO - 💬 收到用户新问题: 'mqtt 有什么优点？'
2025-08-16 19:04:48 - INFO - ✍️ 开始向用户流式传输回答...
2025-08-16 19:04:48 - INFO - HTTP Request: POST https://api.chatanywhere.tech/embeddings "HTTP/1.1 200 OK"
2025-08-16 19:04:48 - INFO - 🔍 向量检索完成，找到 10 个相关文档:
2025-08-16 19:04:48 - INFO -   📄 文档 1 (页码: 3): 'Another important aspect of the protocol is that MQTT is  extremely easy to implement on the client side. Ease of use  w...'
2025-08-16 19:04:48 - INFO -   📄 文档 2 (页码: 56): 'protocol more robust and efficient. Who Needs More Client Feedback While Using  MQTT? Over the years, MQTT has become a ...'
2025-08-16 19:04:48 - INFO -   📄 文档 3 (页码: 51): 'IoT Standard With its rich feature set, MQTT 5 has cemented its place  as the go-to choice for diverse IoT use cases, su...'
2025-08-16 19:04:48 - INFO -   📄 文档 4 (页码: 50): '1. Better Error Handling for More Robust Systems 2. More Scalability for Cloud Native Computing 3. Greater Flexibility a...'
2025-08-16 19:04:48 - INFO -   📄 文档 5 (页码: 51): 'firmware version of the device platform can be added to the  message header, facilitating analysis and processing by the...'
2025-08-16 19:04:48 - INFO -   📄 文档 6 (页码: 50): 'delivered within this time frame, it’s automatically deleted. This  feature is particularly useful in ensuring that netw...'
2025-08-16 19:04:48 - INFO -   📄 文档 7 (页码: 6): 'communication participants all endure in this latest  version. However, several foundational mechanics have  been added ...'
2025-08-16 19:04:48 - INFO -   📄 文档 8 (页码: 3): 'supports persistent sessions between devices and  servers, enhancing message reliability by ensuring  that messages are ...'
2025-08-16 19:04:48 - INFO -   📄 文档 9 (页码: 2): 'Chapter 1: Introduction to MQTT MQTT is a lightweight messaging protocol originally designed for communication in constr...'
2025-08-16 19:04:48 - INFO -   📄 文档 10 (页码: 45): 'objectives for MQTT 5, aimed at enhancing scalability,  formalizing common patterns such as capability discovery and  re...'
2025-08-16 19:04:53 - INFO - HTTP Request: POST https://api.chatanywhere.tech/chat/completions "HTTP/1.1 200 OK"
2025-08-16 19:04:54 - INFO - ✅ 流式传输完成
```

可以看到，回答过程中确实检索了向量数据库。

到此一个完整RAG项目就完成了！

🎉🎉🎉
