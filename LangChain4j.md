---
title: LangChain4j
date: 2024-08-09 11:19:15
tags:
---

# Memory

就是把之前的对话附上去，设定一个轮数的上限

# RAG

## maven

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-spring-boot-starter</artifactId>
    <version>0.33.0</version>
</dependency>

<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai-spring-boot-starter</artifactId>
    <version>0.33.0</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-pgvector</artifactId>
    <version>0.33.0</version>
</dependency>

<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <version>1.19.7</version>
</dependency>

<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-embeddings-all-minilm-l6-v2</artifactId>
    <version>0.33.0</version>
</dependency>
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-core</artifactId>
    <version>0.33.0</version>
    <scope>compile</scope>
</dependency>
```

## pgvector配置类

```java
@Bean
public PostgreSQLContainer<?> postgreSQLContainer() {
    DockerImageName dockerImageName = DockerImageName.parse("pgvector/pgvector:pg16");
    PostgreSQLContainer<?> postgreSQLContainer = new PostgreSQLContainer<>(dockerImageName);
    postgreSQLContainer.start();
    return postgreSQLContainer;
}

@Bean
public EmbeddingStore<TextSegment> embeddingStore(PostgreSQLContainer<?> postgreSQLContainer) {
    return PgVectorEmbeddingStore.builder()
            .host(postgreSQLContainer.getHost())
            .port(postgreSQLContainer.getFirstMappedPort())
            .database(postgreSQLContainer.getDatabaseName())
            .user(postgreSQLContainer.getUsername())
            .password(postgreSQLContainer.getPassword())
            .table("test")
            .dimension(384)
            .build();
}
```

## 文档转换向量

```java
// 添加文档内容到Embedding Store
TextSegment segment1 = TextSegment.from(Document.txt1);
Embedding embedding1 = embeddingModel.embed(segment1).content();
embeddingStore.add(embedding1, segment1);
```

## 使用

```java
EmbeddingModel embeddingModel = new AllMiniLmL6V2EmbeddingModel();

Embedding queryEmbedding = embeddingModel.embed(message).content();
List<EmbeddingMatch<TextSegment>> relevant = embeddingStore.findRelevant(queryEmbedding, 1);
EmbeddingMatch<TextSegment> embeddingMatch = relevant.get(0);
return assistant.chat(embeddingMatch.embedded().text() + message);
```

# 向量数据库

LangChain4j支持的向量数据库

https://docs.langchain4j.dev/integrations/embedding-stores/

## pgvector

使用pgvector作为向量数据库

```shell
docker run --name pgvector-container -e POSTGRES_PASSWORD=mysecretpassword -d postgres:latest
# 进入容器
docker exec -it pgvector-container bash

apt-get update
apt-get install -y postgresql-server-dev-all gcc make git
git clone https://github.com/pgvector/pgvector.git
cd pgvector
make
make install
# 进入docker的pg命令行
docker exec -it pgvector-container psql -U postgres
CREATE EXTENSION vector;
# 用于验证vector的安装
SELECT * FROM pg_available_extensions WHERE name = 'vector';

```

