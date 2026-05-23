# Invoice Management System

A lightweight, open-source invoice and quotation generation system built with **Django** and **Python**. Perfect for small businesses and freelancers.

## 🎯 Features

✅ **Invoice & Quotation Management**
  - Create, edit, and delete invoices
  - Generate quotations with one-click conversion to invoices
  - Auto-generated invoice numbers
  - Multiple invoice status tracking (Draft, Sent, Paid, Overdue)

✅ **Product Inventory Management**
  - Add and manage products with SKU, price, and cost
  - Track inventory levels
  - Calculate profit margins

✅ **Customer Management**
  - Store customer details (name, email, phone, address)
  - Customer-specific invoice history
  - Quick customer lookup

✅ **Dashboard & Analytics**
  - Total revenue, outstanding, and paid invoices
  - Monthly revenue trends
  - Top customers analysis
  - Profit margin tracking
  - Recent invoices overview

✅ **PDF Generation**
  - Professional invoice PDFs
  - Quotation PDFs
  - Email invoices directly

✅ **User Authentication**
  - Secure user registration and login
  - User-specific data isolation

## 📋 Tech Stack

- **Backend**: Django 4.2
- **Frontend**: Bootstrap 5, HTML/CSS
- **Database**: SQLite (default) / PostgreSQL
- **PDF Generation**: WeasyPrint
- **Forms**: Django Crispy Forms
- **Python Version**: 3.8+

## 🚀 Installation Steps

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/varunaradhya/invoice-system.git
cd invoice-system
```

### Step 2: Create Virtual Environment

#### On Linux/Mac:
```bash
python3 -m venv venv
source venv/bin/activate
```

#### On Windows:
```bash
python -m venv venv
venv\\Scripts\\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` and update settings as needed:
```
SECRET_KEY=your-secret-key-here
DEBUG=True
DATABASE_NAME=db.sqlite3
```

### Step 5: Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### Step 6: Create Superuser (Admin Account)

```bash
python manage.py createsuperuser
```

Follow the prompts to create an admin account.

### Step 7: Collect Static Files

```bash
python manage.py collectstatic --noinput
```

### Step 8: Run Development Server

```bash
python manage.py runserver
```

The application will be available at: **http://localhost:8000**

## 📝 Initial Setup

1. **Login to Admin Panel**: http://localhost:8000/admin
   - Use the superuser credentials you created

2. **Add Business Information**:
   - Navigate to Settings section
   - Update your business details

3. **Add Products**:
   - Go to Products section
   - Click "Add Product"
   - Fill in product details (name, SKU, price, cost)

4. **Add Customers**:
   - Go to Customers section
   - Click "Add Customer"
   - Fill in customer information

5. **Create Your First Invoice**:
   - Go to Invoices section
   - Click "Create Invoice"
   - Select customer and products
   - Set due date and generate

## 📊 Dashboard

Access the dashboard at: **http://localhost:8000/dashboard/**

The dashboard provides:
- Total revenue (all paid invoices)
- Outstanding amount (unpaid invoices)
- Monthly revenue chart
- Top customers by revenue
- Recent invoices
- Profit analysis

## 🔗 API Endpoints (Optional)

If using Django REST Framework:

```
GET    /api/invoices/              - List invoices
POST   /api/invoices/              - Create invoice
GET    /api/invoices/<id>/         - Get invoice details
GET    /api/invoices/<id>/pdf/     - Download invoice PDF
GET    /api/customers/             - List customers
POST   /api/customers/             - Create customer
GET    /api/products/              - List products
GET    /api/dashboard/             - Dashboard analytics
```

## 📁 Project Structure

```
invoice-system/
├── manage.py
├── requirements.txt
├── .env.example
├── README.md
├── LICENSE
├── config/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── invoicing/
│   ├── models.py           # Database models
│   ├── views.py            # View logic
│   ├── urls.py             # URL routing
│   ├── forms.py            # Django forms
│   ├── admin.py            # Admin configuration
│   ├── utils.py            # Utility functions (PDF, email)
│   ├── migrations/
│   ├── templates/
│   │   ├── base.html
│   │   ├── dashboard.html
│   │   ├── invoice_list.html
│   │   ├── invoice_detail.html
│   │   ├── invoice_form.html
│   │   ├── invoice_pdf.html
│   │   ├── customer_list.html
│   │   ├── customer_form.html
│   │   ├── product_list.html
│   │   └── product_form.html
│   └── static/
│       ├── css/
│       └── js/
└── media/                  # User uploads
```

## ⚙️ Configuration

### Using PostgreSQL (Optional)

1. Install psycopg2: `pip install psycopg2-binary`
2. Update `.env`:
   ```
   DATABASE_ENGINE=django.db.backends.postgresql
   DATABASE_NAME=invoice_db
   DATABASE_USER=postgres
   DATABASE_PASSWORD=your_password
   DATABASE_HOST=localhost
   DATABASE_PORT=5432
   ```
3. Run migrations again

### Email Configuration

To enable email notifications:

1. Update `.env` with your email credentials
2. Use Gmail app passwords for security
3. Invoices can be emailed directly from the system

## 🐛 Troubleshooting

### "ModuleNotFoundError: No module named 'django'"
- Ensure virtual environment is activated
- Run `pip install -r requirements.txt`

### "PermissionError" when generating PDFs
- Ensure WeasyPrint is properly installed
- On Linux, you may need: `sudo apt-get install python3-cffi python3-brlapi`

### Database errors
- Delete `db.sqlite3` and migrations (except `__init__.py`)
- Run `python manage.py makemigrations && python manage.py migrate`

## 📚 Usage Examples

### Creating an Invoice Programmatically

```python
from invoicing.models import Invoice, Customer, Product
from datetime import datetime, timedelta

customer = Customer.objects.get(pk=1)
invoice = Invoice.objects.create(
    customer=customer,
    invoice_number='INV-001',
    invoice_type='invoice',
    due_date=datetime.now() + timedelta(days=30)
)

product = Product.objects.get(pk=1)
invoice_item = InvoiceItem.objects.create(
    invoice=invoice,
    product=product,
    quantity=2,
    unit_price=product.price
)
```

### Generating PDF

```python
from invoicing.utils import generate_invoice_pdf

invoice = Invoice.objects.get(pk=1)
pdf_file = generate_invoice_pdf(invoice, request)
```

## 🚢 Deployment

### Deploy to Heroku

1. Create `Procfile`:
   ```
   web: gunicorn config.wsgi
   ```

2. Create `runtime.txt`:
   ```
   python-3.11.0
   ```

3. Push to Heroku:
   ```bash
   heroku create your-app-name
   git push heroku main
   heroku run python manage.py migrate
   ```

### Deploy to PythonAnywhere

1. Sign up at pythonanywhere.com
2. Upload project files
3. Configure WSGI file
4. Set up virtual environment
5. Reload web app

## 📄 License

This project is licensed under the MIT License - see LICENSE file for details.

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📧 Support

For issues and questions:
- Open an issue on GitHub
- Contact: support@invoicesystem.local

## 🎓 Future Enhancements

- [ ] Multi-currency support
- [ ] Payment gateway integration (Stripe, PayPal)
- [ ] Email invoice automation
- [ ] Recurring invoices
- [ ] Expense tracking
- [ ] Tax calculations
- [ ] Mobile app
- [ ] Advanced reporting
- [ ] API documentation

---

**Made with ❤️ for small businesses and freelancers**
