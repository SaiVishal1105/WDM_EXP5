### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 25-09-2025
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
    import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()
            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(self.documents_matrix, columns=self.index.keys())
        print(df)

    def print_all_terms(self):
        print("All terms in the documents:")
        print(list(self.index.keys()))

    # ---------- Boolean Query Parsing ----------
    def tokenize(self, query):
        """Split query into tokens, keeping parentheses separate."""
        query = query.lower().replace("(", " ( ").replace(")", " ) ")
        return query.split()

    def parse(self, tokens):
        """Recursive descent parser for Boolean queries."""
        def parse_expression(index=0):
            result, index = parse_term(index)
            while index < len(tokens) and tokens[index] in ("and", "or"):
                op = tokens[index]
                right, index = parse_term(index + 1)
                if op == "and":
                    result = result & right
                elif op == "or":
                    result = result | right
            return result, index

        def parse_term(index):
            token = tokens[index]
            if token == "(":
                result, index = parse_expression(index + 1)
                if tokens[index] == ")":
                    index += 1
                return result, index
            elif token == "not":
                right, index = parse_term(index + 1)
                all_docs = set(doc_id for postings in self.index.values() for doc_id in postings)
                return all_docs - right, index
            else:  # token is a term
                return self.index.get(token, set()), index + 1

        result, _ = parse_expression(0)
        return result

    def boolean_search(self, query):
        tokens = self.tokenize(query)
        return self.parse(tokens)


# ---------------- MAIN PROGRAM ----------------
documents = {
    1: "Python is a programming language",
    2: "Information retrieval deals with finding information",
    3: "Boolean models are used in information retrieval"
}

indexer = BooleanRetrieval()

# Build index
for doc_id, text in documents.items():
    indexer.index_document(doc_id, text)

# Build matrix
indexer.create_documents_matrix(documents)
indexer.print_documents_matrix_table()
indexer.print_all_terms()

# Query
query = input("Enter your boolean query: ")
results = indexer.boolean_search(query)
if results:
    print(f"Results for '{query}': {results}")
else:
    print("No results found for the query.")

```

### Output:
<img width="1782" height="366" alt="image" src="https://github.com/user-attachments/assets/deba27f7-556b-47f1-8984-d7e416258ae2" />
<img width="1778" height="364" alt="image" src="https://github.com/user-attachments/assets/17d6a83f-92f9-4d3b-9769-473337f866ca" />


### Result:
Implementation of Information Retrieval Using Boolean Model in Python is successfully completed.
