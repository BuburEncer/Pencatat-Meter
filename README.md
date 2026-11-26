# Water Meter Recording System - Tirta Musi

A web app for recording water meter readings. It uses OCR (Optical Character Recognition) to read meter serial numbers and a QR code scanner to identify customers.

## Features

- Login and registration for meter readers
- Customer identification by QR code
- Meter serial number reading with the camera (OCR)
- Recording of start reading, end reading, and usage in cubic meters
- Usage history for the last 3 months
- End reading estimated from the 3-month average
- Per-month status of which customers have and have not been recorded
- Upload of meter and house photos

## Tech stack

### Backend

- Flask (Python web framework)
- MySQL + PyMySQL (database)
- SQLAlchemy (ORM)
- OpenCV + Pytesseract (OCR)
- Flask-CORS (cross-origin support)

### Frontend

- React + Vite
- React Router (navigation)
- Axios (HTTP client)
- html5-qrcode (QR scanner)
- react-easy-crop (image cropping)
- Tesseract.js (in-browser OCR)

## Project structure

```
sistem-pencatatan-meter/
├── Backend/
│   ├── routes/              # API endpoints
│   │   ├── __init__.py
│   │   ├── auth_api.py      # Authentication endpoints
│   │   ├── pelanggan_api.py # Customer data endpoints
│   │   ├── pemakaian_api.py # Usage recording endpoints
│   │   └── seri_meter.py    # Meter serial OCR endpoints
│   ├── utils/               # Helper functions
│   │   ├── __init__.py
│   │   └── image_processing.py
│   ├── database/            # Database files
│   │   ├── meter_tirta_musi (4).sql  # Database schema & sample data
│   │   └── README.md        # Database documentation
│   ├── uploads/             # Uploaded images (meter & house photos)
│   ├── app.py               # Application factory & main entry point
│   ├── config.py            # Configuration settings
│   ├── db.py                # Database connection & initialization
│   ├── models.py            # SQLAlchemy database models
│   ├── requirements.txt     # Python dependencies
│   ├── .env.example         # Environment variables template
│   └── .gitignore           # Git ignore rules for backend
│
├── Frontend/
│   ├── src/
│   │   ├── components/      # Reusable components
│   │   │   ├── ui/          # UI components library
│   │   │   │   ├── Alert.jsx
│   │   │   │   ├── Badge.jsx
│   │   │   │   ├── Button.jsx
│   │   │   │   ├── Card.jsx
│   │   │   │   ├── ImageViewer.jsx
│   │   │   │   ├── Input.jsx
│   │   │   │   ├── Modal.jsx
│   │   │   │   └── index.js
│   │   │   ├── QrGenerator.jsx
│   │   │   ├── QrScanner.jsx
│   │   │   └── SeriScanner.jsx
│   │   ├── pages/           # Page components
│   │   │   ├── Login.jsx
│   │   │   ├── Signup.jsx
│   │   │   ├── ScanPage.jsx
│   │   │   ├── InputKubik.jsx
│   │   │   └── PelangganStatus.jsx
│   │   ├── layouts/         # Layout components
│   │   │   ├── AuthLayout.jsx
│   │   │   └── MainLayout.jsx
│   │   ├── hooks/           # Custom React hooks
│   │   │   ├── useAuth.js
│   │   │   └── useFetch.js
│   │   ├── utils/           # Helper functions
│   │   │   ├── cropImage.js
│   │   │   └── normalizeImage.js
│   │   ├── constants/       # Constants and configs
│   │   │   └── index.js
│   │   ├── api.js           # API configuration & Axios setup
│   │   ├── App.jsx          # Main app component
│   │   ├── App.css          # App styles
│   │   ├── main.jsx         # React entry point
│   │   ├── index.css        # Global styles
│   │   ├── CameraCapture.jsx # Camera capture component
│   │   └── ImageCropper.jsx  # Image cropping component
│   ├── public/              # Static assets
│   │   ├── clock-history.svg
│   │   ├── water-outline-svgrepo-com.svg
│   │   ├── water.svg
│   │   └── vite.svg
│   ├── package.json         # Node dependencies
│   ├── package-lock.json    # Locked dependencies
│   ├── vite.config.js       # Vite configuration
│   ├── tailwind.config.js   # Tailwind CSS configuration
│   ├── postcss.config.js    # PostCSS configuration
│   ├── eslint.config.js     # ESLint configuration
│   ├── .env.example         # Environment variables template
│   ├── .gitignore           # Git ignore rules for frontend
│   └── index.html           # HTML entry point
│
├── uploads/                 # Global uploads folder
├── README.md                # Project documentation (this file)
├── INSTALLATION.md          # Detailed installation guide
├── LICENSE                  # License information
└── .gitignore               # Root git ignore rules
```

## Setup and installation

### Prerequisites

1. Python 3.8+
2. Node.js 16+ and npm
3. MySQL Server
4. Tesseract OCR (for backend OCR)
   - Windows: download from [GitHub](https://github.com/UB-Mannheim/tesseract/wiki)
   - Linux: `sudo apt-get install tesseract-ocr`
   - Mac: `brew install tesseract`

### Backend setup

1. Clone the repository and go to the Backend folder:

   ```bash
   cd Backend
   ```

2. Create a virtual environment:

   ```bash
   python -m venv venv
   ```

3. Activate the virtual environment:
   - Windows:
     ```bash
     venv\Scripts\activate
     ```
   - Linux/Mac:
     ```bash
     source venv/bin/activate
     ```

4. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

5. Create the MySQL database:

   ```sql
   CREATE DATABASE meter_tirta_musi;
   ```

6. Configure environment variables:

   ```bash
   cp .env.example .env
   ```

   Edit `.env` as needed:

   ```env
   SECRET_KEY=your-secret-key-here
   DB_USER=root
   DB_PASSWORD=your_password
   DB_HOST=localhost
   DB_NAME=meter_tirta_musi

   # If Tesseract is not on PATH, set it manually:
   # TESSERACT_CMD=C:\Program Files\Tesseract-OCR\tesseract.exe
   ```

7. Run the app:

   ```bash
   python app.py
   ```

   The backend runs at `http://localhost:5000`.

### Frontend setup

1. Go to the Frontend folder:

   ```bash
   cd Frontend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Configure environment variables:

   ```bash
   cp .env.example .env
   ```

   Edit `.env`:

   ```env
   VITE_API_BASE=http://localhost:5000
   ```

4. Run the app:

   ```bash
   npm run dev
   ```

   The frontend runs at `http://localhost:5173`.

## Database schema

### Table: `pelanggan` (customers)

- `id` (INT, PK)
- `nomor_seri_meter` (VARCHAR, UNIQUE)
- `nama_pelanggan` (VARCHAR)
- `alamat` (TEXT)
- `golongan` (VARCHAR)

### Table: `pemakaian_meter` (meter usage)

- `id` (INT, PK)
- `pelanggan_id` (INT, FK)
- `nomor_seri_meter` (VARCHAR)
- `meter_awal` (INT)
- `meter_akhir` (INT)
- `pemakaian_kubik` (INT)
- `periode_bulan` (INT)
- `periode_tahun` (INT)
- `foto_meteran` (VARCHAR)
- `foto_rumah` (VARCHAR)
- `tanggal_catat` (DATETIME)
- `petugas` (VARCHAR)
- `keterangan` (TEXT)
- UNIQUE: `pelanggan_id`, `periode_bulan`, `periode_tahun`

### Table: `pencatat_meter` (meter readers)

- `id` (INT, PK)
- `username` (VARCHAR, UNIQUE)
- `email` (VARCHAR, UNIQUE)
- `password` (VARCHAR, hashed)
- `created_at` (DATETIME)
- `updated_at` (DATETIME)

## API endpoints

### Authentication

- `POST /api/auth/signup` - register a new meter reader
- `POST /api/auth/login` - log in
- `POST /api/auth/logout` - log out
- `GET /api/auth/me` - get the current user

### Customers

- `GET /api/pelanggan/id/:id` - get a customer by ID
- `GET /api/pelanggan/seri/:nomor_seri` - get a customer by meter serial number

### Usage

- `POST /api/pemakaian` - add a usage record
- `GET /api/pemakaian/:pelanggan_id/last-3` - get the last 3 months
- `GET /api/pemakaian/:pelanggan_id/avg-3` - get the 3-month average
- `GET /api/pemakaian/status-bulan-ini` - recording status for the current month

### OCR

- `POST /api/seri-meter` - read the meter serial number from a photo

### Static files

- `GET /uploads/:filename` - serve uploaded images

## Usage

1. Log in with a meter reader account.
2. Scan the customer's QR code or enter the customer ID.
3. Scan the meter serial number with the camera, or type it in.
4. Enter the usage data:
   - Fill in the start and end readings, or use the 3-month average.
   - Upload the meter and house photos.
5. Save the usage record.
6. Check recording progress on the customer status page.

## Development

### Backend

```bash
cd Backend
source venv/bin/activate  # or venv\Scripts\activate on Windows
python app.py
```

### Frontend

```bash
cd Frontend
npm run dev
```

### Production build

```bash
cd Frontend
npm run build
```

## Troubleshooting

### Tesseract error

If OCR does not work, check that:

1. Tesseract is installed.
2. The Tesseract path in `.env` is correct (if you set one).
3. Setting it manually helps: `TESSERACT_CMD=C:\Program Files\Tesseract-OCR\tesseract.exe`

### Database connection error

1. Check that the MySQL server is running.
2. Check the database credentials in `.env`.
3. Check that the database exists.

### CORS error

1. Check that `CORS_ORIGINS` in the backend `.env` matches the frontend URL.
2. Default: `http://localhost:5173,http://localhost:3000`

## License

Private project - Tirta Musi

## Contributors

- Developer Team

Change `SECRET_KEY` and the database credentials before deploying to production.
