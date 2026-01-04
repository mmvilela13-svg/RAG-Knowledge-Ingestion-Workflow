# RAG-Knowledge-Ingestion-Workflow

# Overview
This workflow is used to feed and maintain the knowledge base of the chatbot with company information using RAG (Retrieval-Augmented Generation) memory.

Its main purpose is to keep the chatbot’s knowledge up to date, consistent, and searchable, ensuring that newer information replaces older content when the same topic is detected.

All data is stored in Supabase, using vector embeddings for semantic search.


# Key Capabilities
	•	Company Knowledge Ingestion
	•	Ingests internal company information to be used by the chatbot.
	•	Enables accurate, contextual responses through RAG memory.
	•	Metadata-Based Content Replacement
	•	If new information refers to an existing topic, the workflow replaces the previous content.
	•	Metadata is used to identify and match related topics, avoiding duplicated or outdated knowledge.
	•	Vector Storage with Supabase
	•	All processed content is stored in Supabase using vector embeddings.
	•	Supports semantic similarity search for chatbot responses.
	•	Chunking Strategy
	•	Information is split into chunks before being embedded.
	•	Improves retrieval accuracy and performance during RAG queries.
