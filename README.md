# An-lise-de-Documentos-Anti-fraude-com-AzureAI
pip install azure-ai-formrecognizer
import os
from azure.ai.formrecognizer import DocumentAnalysisClient
from azure.core.credentials import AzureKeyCredential

# Configurações do Azure
endpoint = "YOUR_AZURE_FORM_RECOGNIZER_ENDPOINT"  # substitua pela sua URL do endpoint
api_key = "YOUR_AZURE_FORM_RECOGNIZER_API_KEY"    # substitua pela sua chave de API

# Criar cliente do Form Recognizer
credential = AzureKeyCredential(api_key)
client = DocumentAnalysisClient(endpoint=endpoint, credential=credential)

# Caminho para o documento
document_path = "path/to/your/document.pdf"  # substitua pelo caminho do seu documento

# Ler o documento
with open(document_path, "rb") as document:
    poller = client.begin_analyze_document("prebuilt-document", document)
    result = poller.result()

# Analisar resultados
for page in result.pages:
    print(f"--- Página {page.page_number} ---")
    for line in page.lines:
        print(f"Texto: {line.content} (Confiança: {line.confidence})")
    print()

# Exibir informações extraídas (opcional)
for table in result.tables:
    for cell in table.cells:
        print(f"Célula [row: {cell.row_index}, col: {cell.column_index}]: {cell.content} (Confiança: {cell.confidence})")
