# Al-Noor Islamic Center - Ramadan Pledges

A Flask-based donation platform for Al-Noor Islamic Center's Ramadan pledge campaign, integrated with Stripe for secure payment processing.

## Features

- **Unit-based or Custom Donations**: Choose between fixed units ($1,000 per unit) or custom donation amounts
- **100 Units Available**: Support Al-Noor Islamic Center with up to 100 donation units
- **Flexible Payment Options**:
  - One-time donations
  - Weekly recurring payments (1-26 weeks)
  - Monthly recurring payments (1-6 months)
- **Multi-currency Support**: Accept donations in USD, CAD, EUR, and GBP
- **Zakat Tracking**: Option to specify if donation includes zakat and the amount
- **Dedication Options**: Dedicate donations to loved ones
- **Secure Payment Processing**: Powered by Stripe Checkout
- **Real-time Unit Tracking**: Live updates on available donation units

## Project Structure

```
al-noor-pledges/
├── app.py                 # Flask backend application
├── static/
│   ├── index.html        # Main donation form
│   ├── thank-you.html    # Success page
│   └── error.html        # Cancellation page
├── requirements.txt       # Python dependencies
├── Procfile              # Deployment configuration
└── README.md             # This file
```

## Setup

### Prerequisites

- Python 3.7+
- Stripe account (get your API keys from [Stripe Dashboard](https://dashboard.stripe.com/apikeys))

### Installation

1. **Clone or download this repository**

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Create a `.env` file** in the project root with your Stripe keys:
   ```env
   STRIPE_SECRET_KEY=sk_test_...
   STRIPE_WEBHOOK_SECRET=whsec_...
   BASE_URL=http://localhost:4242
   ```

4. **Run the application**:
   ```bash
   python app.py
   ```

5. **Open your browser** and navigate to:
   ```
   http://localhost:4242
   ```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `STRIPE_SECRET_KEY` | Your Stripe secret key | Yes |
| `STRIPE_WEBHOOK_SECRET` | Stripe webhook signing secret | Yes (for production) |
| `BASE_URL` | Base URL for success/cancel redirects | No (defaults to localhost:4242) |
| `FLASK_ENV` | Set to `production` in production | No |
| `PORT` | Server port | No (defaults to 4242) |

## Configuration

### Pledge Settings

You can modify these constants in `app.py`:

```python
TOTAL_UNITS = 100          # Total units available
UNIT_PRICE = 1000          # Price per unit in dollars
MIN_UNITS = 1              # Minimum units per pledge
MAX_UNITS = 100            # Maximum units per pledge
```

### Currency Support

By default, the form supports: USD, CAD, EUR, GBP

To add more currencies, update the currency dropdown in `static/index.html` and ensure proper handling in `app.py`.

## Deployment

### Heroku

1. Create a new Heroku app:
   ```bash
   heroku create your-app-name
   ```

2. Set environment variables:
   ```bash
   heroku config:set STRIPE_SECRET_KEY=sk_live_...
   heroku config:set STRIPE_WEBHOOK_SECRET=whsec_...
   heroku config:set BASE_URL=https://your-app-name.herokuapp.com
   heroku config:set FLASK_ENV=production
   ```

3. Deploy:
   ```bash
   git push heroku main
   ```

### Other Platforms

The application is compatible with any platform that supports Python web apps (DigitalOcean, AWS, etc.). Ensure you:
- Install dependencies from `requirements.txt`
- Set required environment variables
- Use Gunicorn as the WSGI server (configured in `Procfile`)

## Stripe Webhook Setup

1. In your Stripe Dashboard, go to **Developers > Webhooks**
2. Add endpoint: `https://your-domain.com/webhook`
3. Select events to listen for:
   - `checkout.session.completed`
   - `invoice.paid`
4. Copy the webhook signing secret to your `.env` file

## Unit Tracking

The application tracks remaining donation units using a JSON file (`/tmp/units_data.json`). Units are decremented immediately when a checkout session is created.

**Note**: For production, consider using a persistent database instead of file-based storage.

## Testing

Use Stripe's test mode with test card numbers:
- Successful payment: `4242 4242 4242 4242`
- Requires authentication: `4000 0025 0000 3155`
- Declined: `4000 0000 0000 9995`

More test cards: [Stripe Testing](https://stripe.com/docs/testing)

## Support

For questions or issues:
- Review [Stripe Documentation](https://stripe.com/docs)
- Check [Flask Documentation](https://flask.palletsprojects.com/)

## License

This project is created for Al-Noor Islamic Center's donation campaigns.

---

**May Allah accept all donations and bless all donors. Ameen.**
