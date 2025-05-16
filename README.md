# QI_AWS_S3

## Descrizione

`S3Client` è una classe Python che semplifica l'interazione con AWS S3 per il caricamento, la gestione e la cancellazione di file. Supporta upload di immagini, DataFrame, PDF e la generazione di URL pre-firmati.

## Installazione

Assicurati di avere installato i seguenti pacchetti:

```bash
pip install boto3 aioboto3 pandas matplotlib fpdf
```

## Utilizzo

### Inizializzazione

Puoi inizializzare la classe passando le credenziali AWS come parametri o tramite variabili d'ambiente:

```python
from s3_client import S3Client

s3 = S3Client(
    aws_access_key_id="YOUR_KEY",
    aws_secret_access_key="YOUR_SECRET",
    region_name="YOUR_REGION",
    bucket_name="YOUR_BUCKET"
)
```

### Metodi principali

#### 1. `upload_image(fig, object_name, mimetypes="image/svg+xml", format_file="svg")`

Carica una figura (ad esempio Matplotlib o Plotly) su S3 come immagine (svg, png, jpeg, html).

```python
url = s3.upload_image(fig, "folder/image.svg", mimetypes="image/svg+xml", format_file="svg")
```

#### 2. `upload_from_dataframe(df, object_name, mimetypes="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")`

Carica un DataFrame su S3 come file Excel, CSV o PDF.

```python
url = s3.upload_from_dataframe(df, "folder/data", mimetypes="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
```

#### 3. `upload_to_pdf(text, object_name, mimetypes="application/pdf")`

Esporta del testo in PDF e lo carica su S3.

```python
url = s3.upload_to_pdf("Testo da esportare", "folder/file.pdf")
```

#### 4. `await delete_all(filter=None)`

Cancella tutti i file dal bucket, opzionalmente filtrando per nome.

```python
import asyncio
await s3.delete_all(filter="folder/")
```

## Note

- Tutti i metodi di upload restituiscono una URL pre-firmata per il download del file.
- Gestione degli errori integrata con logging.
- Per l'upload di immagini e DataFrame sono richieste funzioni di utilità (`bytes_from_figure`, `html_from_figure`).

## Esempio completo

```python
import matplotlib.pyplot as plt
import pandas as pd

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [4, 5, 6])

df = pd.DataFrame({"a": [1, 2], "b": [3, 4]})

s3 = S3Client(bucket_name="my-bucket")
img_url = s3.upload_image(fig, "test.svg")
df_url = s3.upload_from_dataframe(df, "mydata")
pdf_url = s3.upload_to_pdf("Hello PDF", "hello.pdf")
```