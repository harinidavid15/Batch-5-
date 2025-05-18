# Source code 
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Sample user data
users = [
    {
        "name": "Alice",
        "email": "alice@example.com",
        "interests": ["laptops", "smartphones"],
        "recent_views": ["Dell XPS 13", "iPhone 15"]
    },
    {
        "name": "Bob",
        "email": "bob@example.com",
        "interests": ["gaming", "headphones"],
        "recent_views": ["Logitech G Pro", "Sony WH-1000XM5"]
    }
]

# Simple product recommendation logic
product_catalog = {
    "laptops": ["MacBook Air M2", "Dell XPS 13", "Lenovo ThinkPad"],
    "smartphones": ["iPhone 15", "Samsung Galaxy S24"],
    "gaming": ["Xbox Series X", "Logitech G Pro"],
    "headphones": ["Sony WH-1000XM5", "Bose QuietComfort 45"]
}

def get_recommendations(interests):
    recommendations = []
    for interest in interests:
        recommendations.extend(product_catalog.get(interest, []))
    return recommendations[:3]  # Limit to 3 items

# Email setup (use real credentials in production)
SMTP_SERVER = 'smtp.example.com'
SMTP_PORT = 587
EMAIL = 'your_email@example.com'
PASSWORD = 'your_password'

def send_email(user):
    message = MIMEMultipart()
    message["From"] = EMAIL
    message["To"] = user["email"]
    message["Subject"] = f"Hi {user['name']}, your personalized product picks!"

    recommendations = get_recommendations(user["interests"])
    html = f"""
    <html>
        <body>
            <h2>Hello {user['name']}!</h2>
            <p>Based on your interests, here are some recommendations:</p>
            <ul>
                {''.join(f'<li>{item}</li>' for item in recommendations)}
            </ul>
        </body>
    </html>
    """

    message.attach(MIMEText(html, "html"))

    with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
        server.starttls()
        server.login(EMAIL, PASSWORD)
        server.send_message(message)
        print(f"Email sent to {user['email']}")
