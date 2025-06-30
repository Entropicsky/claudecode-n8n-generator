<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# how do I use Supabase as a Vector Store for my AI Agent in n8n Cloud. I will be importing the setup via JSON so give me the details on setting up the JSON and the Credentials as well as other setup

To use Supabase as a Vector Store for your AI Agent in n8n Cloud and import the setup via JSON, you need to configure three core components: your Supabase database (with the correct schema), your n8n Supabase credentials, and the JSON workflow that uses the Supabase Vector Store node. Here’s a step-by-step guide with the technical details:

## 1. Supabase Database Setup

Before you import anything into n8n, your Supabase project must have a table ready for storing vectors:

- **Enable pgvector**: Ensure the `pgvector` extension is enabled in your Supabase PostgreSQL database.
- **Create the Table**: Use the Supabase SQL Editor to run a script like this (adjust the vector dimension to match your embedding model, e.g., 768, 1024, or 3072):

```sql
create extension if not exists vector;

create table if not exists documents (
  id bigserial primary key,
  content text,
  metadata jsonb,
  embedding vector(768)
);
```

- `content`: Text chunk from your documents.
- `metadata`: JSON for extra info (source, timestamp, etc.).
- `embedding`: Vector field for the embedding[^1][^2][^3].


## 2. n8n Supabase Credentials

- **Host**: Your Supabase API URL (from Project Settings > API, "Project URL").
- **Service Role Secret**: The `service_role` API key (from Project Settings > API, "Project API Keys")[^4][^5][^3].

**How to Add Credentials in n8n Cloud:**

- Go to n8n Cloud > Credentials > New Credential > Supabase.
- Enter the Host (API URL) and Service Role Secret.
- Test the connection to ensure it works[^4][^5].


## 3. n8n Workflow JSON (Import Template)

Here’s a minimal JSON example for a workflow that upserts data into Supabase as a vector store. You’ll need to adjust the credential name and mapping to your use case.

```json
{
  "nodes": [
    {
      "parameters": {
        "operation": "upsert",
        "collection": "documents",
        "content": "={{ $json[\"content\"] }}",
        "metadata": "={{ $json[\"metadata\"] }}",
        "embedding": "={{ $json[\"embedding\"] }}"
      },
      "id": "SupabaseVectorStore",
      "name": "Supabase Vector Store",
      "type": "n8n-nodes-langchain.vectorstoresupabase",
      "typeVersion": 1,
      "credentials": {
        "supabaseApi": {
          "id": "YOUR_SUPABASE_CREDENTIAL_ID",
          "name": "YOUR_SUPABASE_CREDENTIAL_NAME"
        }
      }
    }
  ],
  "connections": {}
}
```

- Replace `YOUR_SUPABASE_CREDENTIAL_ID` and `YOUR_SUPABASE_CREDENTIAL_NAME` with the actual credential reference from your n8n instance.
- The node type should be `n8n-nodes-langchain.vectorstoresupabase`[^6].
- The `embedding` field should be an array matching your vector dimension (e.g., 768 floats).
- You can connect this node to upstream nodes that generate the content, metadata, and embedding (e.g., from a file parser and an embedding model).


## 4. Importing the Workflow

- In n8n Cloud, go to Workflows > Import.
- Paste your JSON and import.
- After import, open the workflow and double-check the credential assignment for the Supabase Vector Store node.


## 5. Connecting to Your AI Agent

- In your AI Agent node, add the Supabase Vector Store as a tool.
- This enables the agent to perform semantic search over your stored documents for Retrieval-Augmented Generation (RAG) workflows[^6][^3].

**Summary Table**


| Step | What to Do |
| :-- | :-- |
| Supabase Table | Create `documents` table with `content`, `metadata`, `embedding` (vector) columns |
| Credentials in n8n | Add Supabase credential with Host (API URL) and Service Role Secret (service_role key) |
| JSON Workflow | Use/import a node of type `n8n-nodes-langchain.vectorstoresupabase` with correct field mapping |
| Connect to AI Agent | Add Supabase Vector Store as a tool in the AI Agent node |

**References:**

- n8n Supabase Vector Store node documentation[^6]
- Supabase credential setup guide[^4][^5]
- Table schema and RAG setup details[^2][^1][^3]

This setup allows your AI Agent in n8n Cloud to store, retrieve, and search document embeddings using Supabase as a vector database, all orchestrated via your imported JSON workflow.

<div style="text-align: center">⁂</div>

[^1]: https://deepwiki.com/diegoperea20/n8n-rag-gemini-supabase/3.1-supabase-vector-database-setup\&rut=03e2a780f95b63a6978dcac4a4a7c74232c5e4475dab70f7e6d5622fde26b80d

[^2]: https://www.youtube.com/watch?v=jT6BAdj47ZU

[^3]: https://www.nocodesaas.io/p/build-your-own-ai-powered-database

[^4]: https://docs.n8n.io/integrations/builtin/credentials/supabase/

[^5]: https://www.youtube.com/watch?v=3m4n7uWCOB8

[^6]: https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.vectorstoresupabase/

[^7]: https://community.n8n.io/t/impossible-to-filter-a-supabase-vector-store-via-metadata/75895

[^8]: https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.supabase/

[^9]: https://deepwiki.com/diegoperea20/n8n-rag-gemini-supabase/3.2-supabase-authentication

[^10]: https://www.youtube.com/watch?v=oHM5F2f20hs

[^11]: https://community.n8n.io/t/modify-the-match-documents-of-the-supabase-vector-store-for-rag/82608

[^12]: https://community.n8n.io/t/supabase-vector-store-issue/34885

[^13]: https://www.youtube.com/watch?v=snpfbkzkTcI

[^14]: https://community.n8n.io/t/supabase-vector-store-langchain-ai/34627

[^15]: https://github.com/n8n-io/n8n/issues/13813

[^16]: https://blog.csdn.net/lyy2017175913/article/details/144737100

[^17]: https://supabase.com/partners/n8n

[^18]: https://community.n8n.io/t/problem-connecting-to-supabase-with-postgres-credentials/75027

[^19]: https://community.n8n.io/t/supabase-credentials-url-not-being-recognised/124539

[^20]: https://www.reddit.com/r/n8n/comments/1k021tz/help_with_creating_and_call_custom_credentials/

[^21]: https://www.youtube.com/watch?v=dteQC_JC5CM

[^22]: https://dev.to/somprasongd/n8n-credentials-setup-cchadaihkhrb-cchbthukkaarechuuemt-2gc3

[^23]: https://n8n.io/integrations/supabase-vector-store/

[^24]: https://www.youtube.com/watch?v=jakZIZe776Y

[^25]: https://community.n8n.io/t/supabase-vector-store-issue-self-hosted/95974

[^26]: https://www.youtube.com/watch?v=IN9d3426VYY

