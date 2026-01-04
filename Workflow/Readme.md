# Supabase Database Setup

Before running this workflow, the required tables and functions must be created in Supabase.

# Prerequisites
	•	A Supabase project
	•	Access to the SQL Editor
	•	Correct embedding dimensions matching the model used by your chatbot

# SQL Schema

Run the following SQL code in the Supabase SQL Editor:

# code:

-- Enable the pgvector extension to work with embedding vectors

create extension vector;

-- Create a table to store your documents

create table documents (
  id bigserial primary key,
  content text, -- corresponds to Document.pageContent
  metadata jsonb, -- corresponds to Document.metadata
  embedding extensions.vector(1536) -- 1536 works for OpenAI embeddings, change if needed
);

-- Create a function to search for documents

create function match_documents (
  query_embedding extensions.vector(1536),
  match_count int default null,
  filter jsonb DEFAULT '{}'
) returns table (
  id bigint,
  content text,
  metadata jsonb,
  similarity float
)
language plpgsql
as $$
#variable_conflict use_column
begin
  return query
  select
    id,
    content,
    metadata,
    1 - (documents.embedding <=> query_embedding) as similarity
  from documents
  where metadata @> filter
  order by documents.embedding <=> query_embedding
  limit match_count;
end;
$$;

# Important Considerations
	•	Embedding Dimensions
	•	Ensure the vector dimensions (1536 in this example) match the embedding model you are using.
	•	If you change the embedding model, you must update the table and function accordingly.
	•	Metadata Design
	•	Metadata is critical for identifying overlapping topics.
	•	Proper metadata structure allows safe replacement of outdated information.
	•	Data Consistency
	•	This workflow assumes Supabase is the single source of truth for chatbot knowledge.


# Workflow Logic (High Level)
	1.	Receive company information to be ingested.
	2.	Split the content into semantic chunks.
	3.	Generate embeddings for each chunk.
	4.	Check metadata to detect existing topics.
	5.	Replace or insert content accordingly.
	6.	Store the final chunks and embeddings in Supabase.


# Use Cases
	•	Maintaining an up-to-date chatbot knowledge base
	•	Replacing outdated company documentation automatically
	•	Powering RAG-based chatbots with structured, searchable knowledge
