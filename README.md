# 🧾 API Documentation

> Base URL: `http://<your-server-domain>:PORT`
> All requests must include your unique API key either in:

* `x-api-key` header, or
* `api_key` query parameter (only on GETs like `/embed_chat`)

---

## 🔐 Authentication

Most endpoints require authentication via your API key.

### ✅ Required Header

```http
x-api-key: <API_KEY>
```

---

## 📌 1. Register a Company

Used to generate a new API key for your company.

**POST** `/register_company`

### Request Body:

```json
{
  "name": "My Company Name"
}
```

### Response:

```json
{
  "company_id": "uuid",
  "api_key": "<API_KEY>",
  "embed_link": "http://yourdomain.com/embed_chat?api_key=<API_KEY>",
  "embeded_script_tag": "<script src='...'></script>"
}
```

---

## 🔑 2. Login

**POST** `/login`

### Request Body:

```json
{
  "company_id": "<Company_ID>"
}
```

### Response:

```json
{
  "api_key": "<API_KEY>",
  "company_id": "uuid"
}
```

---

## 📤 3. Upload Files

**POST** `/upload`
Use `multipart/form-data`

### Form Data:

* `file`: One or more files (.pdf, .docx, .pptx, .txt, .mp3, .wav, .mp4, .xlsx, .csv, .jpg, etc.)

### Header:

```http
x-api-key: <API_KEY>
```

### Response:

```json
[
  {
    "file": "example.pdf",
    "chunks": 12,
    "upload_id": "abc123",
    "status": "success"
  }
]
```

---

## 📄 4. List Uploaded Files

**GET** `/list_files`

### Header:

```http
x-api-key: <API_KEY>
```

### Response:

```json
{
  "files": [
    {
      "file_name": "example.pdf",
      "file_path": "/uploads/example.pdf",
      "upload_id": "abc123",
      "chunk_count": 12
    }
  ]
}
```

---

## 🗑️ 5. Delete an Upload

**POST** `/delete`

### Header:

```http
x-api-key: <API_KEY>
```

### Request Body:

```json
{
  "upload_id": "abc123"
}
```

### Response:

```json
{
  "message": "Deletion completed for upload_id: abc123"
}
```

---

## 💬 6. Ask a Question (Chat)

**POST** `/chat`

### Header:

```http
x-api-key: <API_KEY>
```

### Request Body:

```json
{
  "message": "What is our refund policy?"
}
```

### Response:

```json
{
  "answer": "Your refund policy is available on page 2 of terms.pdf...",
  "references": [
    {
      "file_name": "terms.pdf",
      "file_path": "/uploads/terms.pdf",
      "chunk_index": 3,
      "upload_id": "abc123"
    }
  ]
}
```

---

## 💡 7. Embed Widget (Chatbot UI)

### A. Full Page Chatbot

**GET** `/embed_chat?api_key=<API_KEY>`

> Embed this URL in an iframe on your website.

### B. Script Embed

**GET** `/embed_script.js?api_key=<API_KEY>`

> Add this `<script>` tag to any webpage to show a floating chatbot:

```html
<script src="http://yourdomain.com/embed_script.js?api_key=<API_KEY>"></script>
```

---

## 📂 8. Serve Uploaded Files

**GET** `/uploads/<filename>`

Returns the raw file (PDF, DOCX, image, etc.)

---

## 🛠️ Setup Notes

* Make sure your `.env` includes the following:

  ```
  AWS_ACCESS_KEY=...
  AWS_SECRET_ACCESS_KEY=...
  AWS_REGION=us-east-1
  DATABASE_URL=...
  MODEL_ID_EMBED=...
  MODEL_ID_CHAT=...
  ```

* Run the Flask app:

  ```bash
  python your_script.py
  ```

---
